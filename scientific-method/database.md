# Database Diagnostics

[Back to the Scientific Method](README.md)

The database is where a lot of backend symptoms end up. It's also one of the most honest layers to investigate, because the engine will tell you exactly what it's doing if you ask: query plans, wait events, lock tables, and statement statistics. The mistakes compound quietly, though. A query that runs fine on 10,000 rows can fall over at 10 million.

## Typical symptoms

- One query or endpoint got slow, often gradually as data grew
- Sudden slowdown after a migration, deploy, or statistics change
- Timeouts or deadlock errors under concurrent load
- "Too many connections" errors
- Reads return stale data from a replica
- Disk or storage growth that outpaces data growth
- Data that violates rules the application assumes (duplicates, orphans, nulls)

## Observation tooling

| Need | PostgreSQL | MySQL / MariaDB | SQL Server | MongoDB |
|---|---|---|---|---|
| Query plan with real timings | `EXPLAIN (ANALYZE, BUFFERS)` | `EXPLAIN ANALYZE` (8.0.18+), `EXPLAIN FORMAT=JSON` | Actual Execution Plan, `SET STATISTICS IO, TIME ON` | `.explain("executionStats")` |
| Top statements by total time | `pg_stat_statements` | `performance_schema`, `sys` schema, slow query log | **Query Store**, `sys.dm_exec_query_stats` | Database profiler, `$currentOp` |
| What's running and waiting now | `pg_stat_activity` (with `wait_event`) | `SHOW PROCESSLIST`, `performance_schema.events_waits_*` | `sys.dm_exec_requests`, `sys.dm_os_wait_stats` | `db.currentOp()` |
| Locks and blocking | `pg_locks` joined to `pg_stat_activity`, `pg_blocking_pids()` | `performance_schema.data_locks`, `SHOW ENGINE INNODB STATUS` | `sys.dm_tran_locks`, blocked process report, deadlock graphs | `currentOp` with lock info |
| Table and index health | `pg_stat_user_tables` (dead tuples, last vacuum), `pg_stat_user_indexes` (unused indexes) | `information_schema` stats, `sys.schema_unused_indexes` | `sys.dm_db_index_usage_stats`, index fragmentation DMVs | `$indexStats` |
| Replication lag | `pg_stat_replication`, `pg_last_xact_replay_timestamp()` | `SHOW REPLICA STATUS` | AG DMVs (`sys.dm_hadr_*`) | `rs.printSecondaryReplicationInfo()` |
| Benchmarking | `pgbench` | `sysbench` | HammerDB | YCSB |

Also useful across engines: your ORM's SQL logging (to see what's actually sent), connection pool metrics from the application, and managed-service insights such as AWS RDS Performance Insights or Azure SQL Query Performance Insight.

## Common hypotheses and how to tell them apart

| Symptom | Candidate hypotheses | Discriminating experiment |
|---|---|---|
| Query got slow as data grew | (a) Missing or unusable index (b) Stale statistics leading to a bad plan (c) N+1 pattern from the ORM (d) Query returns far more rows than needed | `EXPLAIN ANALYZE`: a Seq Scan on a large table, or a big gap between estimated and actual rows. Run `ANALYZE` and re-plan for (b). Count statements per request in ORM logs for (c). Compare rows returned to rows used for (d). |
| Sudden slowdown, no code change | (a) Plan flip after statistics refresh (b) Autovacuum or maintenance running (c) Table bloat (d) Parameter-sensitive plan (parameter sniffing) | Compare current and historical plans (Query Store or `auto_explain`). Check maintenance activity at the time of the spike. Run the query with parameter values that cover typical and skewed cases for (d). |
| Deadlocks under load | (a) Two code paths lock rows in opposite order (b) Missing index turns row locks into wider locks (c) Long transactions holding locks | Read the deadlock graph or the `INNODB STATUS` output and identify both statements. Check lock order in the code. Measure transaction duration. |
| "Too many connections" | (a) Pool per instance × instances > DB max (b) Connection leak (c) Long-idle transactions (`idle in transaction`) | Add up configured pool maxima. Look for `idle in transaction` in `pg_stat_activity`. Watch whether connection count climbs without load. |
| Stale reads | (a) Replica lag (b) App reading from replica right after writing to primary (c) Cache in front of the DB | Measure replica lag at the moment of the stale read. Trace which host served the read. Bypass the cache. |
| Duplicate rows the app says "can't happen" | (a) No unique constraint, check-then-insert race (b) Non-idempotent retries (c) Import or backfill bug | Run concurrent inserts in a test. Match duplicate timestamps against retry logs and job runs. |

## Worked example: an endpoint slowed from 80 ms to 2.5 s over six weeks

1. **Observe.** `GET /customers/{id}/orders` p95 has climbed steadily from 80 ms to 2.5 s over six weeks. There were no deploys to that endpoint in that time. The `orders` table grew from 4M to 11M rows.
2. **Question.** Why has this endpoint's latency grown faster than linearly with the size of `orders`?
3. **Research.** `pg_stat_statements` shows one statement accounts for 92% of this endpoint's DB time: `SELECT ... FROM orders WHERE customer_id = $1 AND status <> 'archived' ORDER BY created_at DESC LIMIT 50`. There's an index on `customer_id` alone.
4. **Hypothesize.**
   - H1: The planner uses the `customer_id` index but then sorts every order for large customers. A few customers now have 200k+ orders.
   - H2: Table bloat from heavy updates makes every scan read many dead tuples.
   - H3: Stale statistics lead the planner to a sequential scan.
5. **Predict.** H1: `EXPLAIN ANALYZE` for a large customer shows an Index Scan on `customer_id` feeding a Sort node with a large row count, and a small customer is fast. H2: `n_dead_tup` is high relative to live tuples, and `BUFFERS` shows many more pages read than rows returned. H3: the plan shows a Seq Scan, and running `ANALYZE` changes it.
6. **Experiment.** On a staging copy restored from last night's snapshot, run `EXPLAIN (ANALYZE, BUFFERS)` for a small customer and a large one. Check `pg_stat_user_tables`. Run `ANALYZE orders` and re-plan.
7. **Analyze.** The plan is already an Index Scan, and `ANALYZE` didn't change it, so H3 is refuted. Dead tuples are 3%, so H2 is refuted. The large customer shows a Sort of 214,000 rows taking 2.3 s, and the small customer takes 4 ms. H1 is supported.
8. **Conclude.** Created `CREATE INDEX CONCURRENTLY ... ON orders (customer_id, created_at DESC) WHERE status <> 'archived'`. The new plan is an Index Scan with no Sort, at 6 ms for the large customer. Added a CI check that runs `EXPLAIN` on the top 20 queries against a seeded dataset with a realistic skew.

## Experiment techniques

- **Test on production-shaped data.** Plans depend on data distribution. A staging database with 1,000 evenly distributed rows will lie to you. Use a restored snapshot (masked if needed) or generate data with realistic skew.
- **Run `EXPLAIN ANALYZE` carefully.** It actually executes the statement. Wrap writes in `BEGIN; ... ROLLBACK;`, and never run it against a production write path casually.
- **Clear or account for the cache.** The first run reads from disk and later runs read from memory. Report both, or use `BUFFERS` to see the difference.
- **Build indexes online.** Use `CREATE INDEX CONCURRENTLY` (Postgres), `ALGORITHM=INPLACE` (MySQL), or `ONLINE = ON` (SQL Server) when testing a fix anywhere with real traffic.
- **Reproduce lock problems with two sessions.** Open two `psql` or client sessions and step through the conflicting statements by hand. It's the clearest way to see a deadlock happen.

## AI prompts that help

- "Here's the `EXPLAIN (ANALYZE, BUFFERS)` output. Where is the time going, where do estimated and actual rows diverge, and what would you test first?"
- "Here's a deadlock graph. Which two statements conflict, in what lock order, and what code paths could produce that order?"
- "Generate a SQL script that seeds 10M rows into this schema with a Zipf-like distribution of `customer_id`."

**Pitfall:** AI will happily suggest adding an index for every slow query. Indexes slow writes and take space, and the wrong column order won't help at all. Confirm with a plan, then check the write-side cost.

## Theory behind the playbook

- [CS11 Database Systems](../computer-science-in-ai-curriculum/talks/11-database-systems.md): query processing, indexing, transactions, and isolation
- [CS02 Data Structures](../computer-science-in-ai-curriculum/talks/02-data-structures.md): B-trees and hash indexes are data structures
- [SDA18 Databases at Scale](../computer-science-software-design-and-architecture/curriculum/part-5/part-5-18--databases-at-scale/README.md): replication, sharding, denormalization, SQL tuning
- [SDA19 NoSQL Database Types](../computer-science-software-design-and-architecture/curriculum/part-5/part-5-19--nosql-database-types/README.md)
- [SDA15 Consistency & Availability Patterns](../computer-science-software-design-and-architecture/curriculum/part-4/part-4-15--consistency-availability-patterns/README.md): replica lag and eventual consistency
- [SDA13 Enterprise Application Patterns](../computer-science-software-design-and-architecture/curriculum/part-3/part-3-13--enterprise-application-patterns/README.md): ORMs, repositories, and where N+1 comes from
