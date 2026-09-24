# Backend Diagnostics

[Back to the Scientific Method](README.md)

Backend problems usually show up as latency, errors, or resource exhaustion. The hard part is that services depend on other services, databases, caches, and queues. A symptom in one service is often a cause somewhere else. Distributed tracing and the RED method make that visible.

## Typical symptoms

- p95/p99 latency rising while the median looks fine
- Error-rate spikes (5xx, timeouts, dependency errors)
- Throughput plateaus even though CPU looks idle
- Memory grows until the process is killed or restarted
- Intermittent wrong results under load (race conditions)
- Everything is slow because one downstream dependency is slow
- Retries amplifying a small outage into a big one

## Observation tooling

| Need | Tools |
|---|---|
| Request rate, errors, duration per endpoint | Metrics via Prometheus + Grafana, Datadog, New Relic, CloudWatch, Azure Monitor, or similar |
| Where time goes across services | **Distributed tracing** with **OpenTelemetry**, viewed in Jaeger, Grafana Tempo, Zipkin, Honeycomb, Datadog APM, or similar |
| Structured, searchable logs | ELK/OpenSearch, Grafana Loki, Splunk, or cloud-native logging. Use correlation or trace IDs in every log line. |
| CPU hot spots | Profilers: `async-profiler` / JFR (JVM), `py-spy` (Python), `pprof` (Go), `dotnet-trace` / `dotnet-counters` (.NET), `--cpu-prof` / Clinic.js (Node) |
| Memory growth | Heap dumps and analyzers: Eclipse MAT (JVM), `tracemalloc` / `memray` (Python), `dotnet-gcdump`, Node heap snapshots in Chrome DevTools |
| Threads, locks, deadlocks | Thread dumps (`jstack`, `jcmd`), `dotnet-stack`, `py-spy dump`, goroutine dumps |
| Reproducing load | **k6**, Gatling, Locust, JMeter, `wrk` / `hey` for quick checks |
| Poking an endpoint by hand | `curl -v`, HTTPie, Postman or Bruno, `grpcurl` for gRPC |
| Finding the change that broke it | `git bisect run` with a test script, deploy markers on dashboards |

## Common hypotheses and how to tell them apart

| Symptom | Candidate hypotheses | Discriminating experiment |
|---|---|---|
| p99 latency up, p50 flat | (a) Contention on a shared resource (lock, connection pool) (b) GC pauses (c) One slow dependency on some requests (d) Noisy neighbor | Compare slow and fast traces side by side and find the span that differs. Graph pool wait time for (a). Correlate GC logs or pause metrics with slow requests for (b). Group slow traces by downstream call for (c). |
| Throughput plateau, CPU idle | (a) Connection pool exhausted (b) Thread pool exhausted (c) Synchronous I/O blocking the event loop (d) Downstream rate limit | Take a thread dump at the plateau and count threads waiting on the pool. Watch pool "active" vs. "max" for (a)/(b). Measure event-loop lag for (c). Check for 429s or throttle headers from dependencies for (d). |
| Memory grows until OOM | (a) Unbounded cache or collection (b) Listener or subscription leak (c) Large payloads buffered in memory (d) Native or off-heap leak | Take two heap dumps N minutes apart and compare dominators for (a)/(b). Correlate growth with request size for (c). If heap is flat but RSS grows, it's (d). |
| Intermittent wrong totals under load | (a) Race on shared mutable state (b) Missing transaction isolation (c) Non-idempotent retry double-applying | Write a concurrency test with N parallel requests on the same entity. Check whether duplicates line up with retry logs for (c). Inspect the isolation level for (b). |
| Error spike after deploy | (a) Code regression (b) Config or secret change (c) Schema or contract mismatch with a dependency (d) Coincidental dependency outage | Roll back one canary instance only and compare error rate. Diff config between versions. Check dependency error rates independently of your service. |
| Small outage became a big one | (a) Retry storm with no backoff (b) No circuit breaker (c) Timeouts longer than the caller's timeout | Graph outbound request rate to the failing dependency during the incident. A multiple of normal traffic means (a). Review timeout and retry config along the call chain. |

## Worked example: throughput plateau at 400 requests per second

1. **Observe.** A load test plateaus at ~400 rps on the orders service. Adding instances doesn't help. CPU is 35% per instance. p99 goes from 120 ms to 4 s at the plateau.
2. **Question.** Why does the orders service stop scaling at ~400 rps with idle CPU?
3. **Research.** Four instances, each with a DB pool max of 25. The database allows 100 connections total. The test adds instances up to 8.
4. **Hypothesize.**
   - H1: Total DB connections are capped at 100, so extra instances just wait for connections.
   - H2: A downstream inventory service rate-limits at 400 rps.
   - H3: A synchronized block in the pricing code serializes requests.
5. **Predict.** H1: pool wait time rises sharply at the plateau, the database shows ~100 active connections, and inventory 429s stay at zero. H2: inventory returns 429s or throttling headers. H3: thread dumps show most threads blocked on the pricing monitor.
6. **Experiment.** Rerun the load test with OpenTelemetry tracing on, capture thread dumps at the plateau, and collect DB connection metrics. Then rerun with the inventory call stubbed.
7. **Analyze.** No 429s, and the stubbed inventory run plateaued at the same point, so H2 is refuted. Thread dumps show 180 threads waiting in `HikariPool.getConnection`, not on the pricing lock, so H3 is refuted. The database reports 100 of 100 connections in use. H1 is supported.
8. **Conclude.** Adding instances split the same 100 connections across more pools. Fixes: put PgBouncer in front of the database in transaction mode, and shorten transaction scope in the order handler. The handler was holding a connection during an HTTP call to inventory. Retest reached 1,100 rps. An alert now fires on pool wait time.

## Experiment techniques

- **Warm up before measuring.** JIT compilation, caches, and connection pools all distort the first minute of a load test.
- **Ramp load in steps.** Step-wise ramps (100, 200, 400, 800 rps) show *where* a curve breaks, which is often more useful than the peak number.
- **Stub dependencies to isolate.** Replace a downstream call with a fixed-latency stub (WireMock, a mock server, or a feature flag). If the symptom disappears, the dependency is involved.
- **Inject faults on purpose.** Add latency or errors to a dependency (Toxiproxy, a service mesh fault injection, or a chaos tool) to check that timeouts, retries, and circuit breakers do what you think.
- **Use a canary as the control group.** Run old and new versions side by side on split traffic and compare RED metrics directly.

## AI prompts that help

- "Here are two traces, one fast and one slow, for the same endpoint. What differs, and what hypotheses does that suggest?"
- "Here's a thread dump taken at our throughput plateau. Group the threads by state and stack, and tell me what most of them are waiting on."
- "Write a k6 script that ramps from 50 to 1,000 rps in steps of 50 every 60 seconds, against these three endpoints with these weights."

**Pitfall:** "Increase the pool size" and "add a retry" are the two most common AI-suggested fixes. Both can make things worse. A bigger pool overloads the database, and a retry without backoff starts a retry storm. Find the mechanism first.

## Theory behind the playbook

- [CS07 Introduction to Computer Systems](../computer-science-in-ai-curriculum/talks/07-introduction-to-computer-systems.md): memory, concurrency, and why races fail silently
- [CS08 Operating Systems](../computer-science-in-ai-curriculum/talks/08-operating-systems.md): threads, scheduling, and synchronization
- [CS10 Distributed Systems](../computer-science-in-ai-curriculum/talks/10-distributed-systems.md): partial failure and timeouts
- [SDA21 Asynchronism](../computer-science-software-design-and-architecture/curriculum/part-6/part-6-21--asynchronism/README.md): back pressure and idempotency
- [SDA23 Performance Antipatterns](../computer-science-software-design-and-architecture/curriculum/part-6/part-6-23--performance-antipatterns/README.md): retry storms, synchronous I/O, improper instantiation
- [SDA24 Monitoring & Observability](../computer-science-software-design-and-architecture/curriculum/part-6/part-6-24--monitoring-observability/README.md)
- [SDA27 Reliability & Resiliency Patterns](../computer-science-software-design-and-architecture/curriculum/part-7/part-7-27--reliability-resiliency-patterns/README.md): circuit breaker, bulkhead, retry
