# The Scientific Method for Software Diagnostics

[Back to the project README](../README.md)

Most debugging goes like this: someone sees a symptom, guesses a cause, changes something, and watches to see if the symptom goes away. When it does, they call it fixed. When it doesn't, they guess again. AI tools make this worse, because guesses now arrive instantly, sound confident, and come with a ready-made patch.

The scientific method replaces guessing with experiments. It is slower for the first ten minutes and much faster for the next ten hours. It also leaves a written record, so the next person doesn't repeat your dead ends.

This folder describes the method once, then applies it to eight areas of a software system, each with its own tooling.

## The loop

```text
  ┌────────────────┐
  │   1. Observe   │◄─────────────────────────────────────────────┐
  └───────┬────────┘                                              │
          ▼                                                       │
  ┌────────────────┐   ┌────────────────┐   ┌────────────────┐    │
  │  2. Question   │──►│  3. Research   │──►│ 4. Hypothesize │    │
  └────────────────┘   └────────────────┘   └───────┬────────┘    │
                                                    ▼             │
  ┌────────────────┐   ┌────────────────┐   ┌────────────────┐    │
  │   7. Analyze   │◄──│ 6. Experiment  │◄──│   5. Predict   │    │
  └───────┬────────┘   └────────────────┘   └────────────────┘    │
          ▼                                                       │
  ┌────────────────────┐   refuted or inconclusive                │
  │ 8. Conclude, share │──────────────────────────────────────────┘
  └────────────────────┘
```

| Step | What you do | What you produce |
|---|---|---|
| **1. Observe** | Collect symptoms without interpreting them. Error text, timestamps, affected users, dashboards, screenshots. | A list of facts, each with its source |
| **2. Question** | Turn the symptoms into one precise question. What fails, where, since when, for whom, how often? | "Why does checkout p95 latency jump from 400 ms to 3 s between 12:00 and 13:00 UTC on weekdays?" |
| **3. Research** | Establish the baseline and what changed. Deploys, config, traffic, dependencies, data volume, known issues. | A timeline and a "normal" to compare against |
| **4. Hypothesize** | Propose a cause that could be proven *wrong*. Write down several before committing to one. | "The lunchtime report job holds row locks on `orders`, and checkout writes wait on them." |
| **5. Predict** | State what you'll see if the hypothesis is true, and what you'll see if it's false. | "If true, `pg_locks` will show checkout sessions waiting on the report job's PID. If false, there will be no lock waits during the spike." |
| **6. Experiment** | Change or measure *one* variable, with a control, in the safest environment that still reproduces the problem. | Raw data: measurements, logs, traces |
| **7. Analyze** | Compare the results to your predictions. Did they support or refute the hypothesis? Could something else explain them? | A verdict: supported, refuted, or inconclusive |
| **8. Conclude & share** | Fix the cause, add a regression test or alert, and write it up. If refuted, go back to step 1 or 4 with what you learned. | A fix, a guard against recurrence, and an experiment log |

## Ground rules

1. **Reproduce before you fix.** If you can't make it happen on demand, you can't prove you fixed it. When reproduction is impossible, instrument production so the next occurrence proves or disproves your hypothesis.
2. **Change one variable at a time.** Two changes at once means you don't know which one mattered, or whether one masked the other.
3. **Measure the baseline first.** "It's slow" means nothing without "compared to what."
4. **Try to disprove, not confirm.** Design experiments that would *kill* your favorite hypothesis. The one that survives is worth trusting.
5. **Write it down as you go.** Use the [experiment log template](experiment-log-template.md). Memory is a bad lab notebook, especially at 2 a.m.
6. **Prefer reversible, low-blast-radius experiments.** Local before staging, staging before a canary, a canary before everyone. Feature flags beat redeploys.
7. **Correlation is a lead, not a verdict.** "It started after the deploy" tells you where to look. It does not tell you what broke.
8. **Timebox each hypothesis.** If an experiment hasn't produced a clear answer in the time you set, step back and question the hypothesis, not just the experiment.

## Writing a good hypothesis

A useful hypothesis names a mechanism, is specific enough to test, and predicts something you can measure.

| Weak | Why it's weak | Strong |
|---|---|---|
| "The database is slow." | Names no mechanism and predicts nothing | "The `orders` query does a sequential scan since the `customer_id` index was dropped in migration 0142. `EXPLAIN ANALYZE` will show a Seq Scan and >90% of the request time." |
| "It's a memory leak." | Can't be refuted as written | "The `ReportCache` map grows without eviction. Heap snapshots 10 minutes apart will show `ReportCache` retained size growing roughly linearly with request count." |
| "Something's wrong with the network." | Too broad to design an experiment around | "Pods in zone `us-east-1c` resolve `payments.internal` to a decommissioned IP. `dig` from those pods will return 10.4.2.17, and from other zones it will return 10.4.9.x." |
| "The AI said it's a race condition." | Borrowed authority, not evidence | "Two workers can claim the same job because `SELECT ... FOR UPDATE` is missing. Running 50 concurrent claims in a test will produce duplicate claims." |

Keep a list of **competing hypotheses**. Rank them by likelihood and by how cheap they are to test. Test cheap ones first, even if they're less likely. Ruling things out is progress.

## Frameworks that make observation systematic

| Framework | Use it for | The questions |
|---|---|---|
| **RED** (Rate, Errors, Duration) | Request-driven services | How many requests? How many fail? How long do they take? |
| **USE** (Utilization, Saturation, Errors), from Brendan Gregg | Resources: CPU, memory, disk, network, pools | How busy is it? Is work queuing? Is it throwing errors? |
| **Four Golden Signals**, from the Google SRE book | User-facing systems | Latency, traffic, errors, saturation |
| **Differential diagnosis** | "Works here, not there" | What's different between the working and failing cases? Version, config, data, region, user, browser, time of day? |
| **Bisection** | "It used to work" | Halve the search space: `git bisect`, feature-flag halves, config halves, date ranges |

## Where AI fits

AI is a good lab assistant and a bad lab. Use it to speed up each step, never to skip steps.

| Step | AI helps by | Watch out for |
|---|---|---|
| Observe | Summarizing long logs, clustering similar errors, explaining unfamiliar stack traces | It can drop the one odd line that matters. Keep the raw data. |
| Question | Tightening a vague symptom into a precise question | Framing the question around its own favorite answer |
| Research | Explaining unfamiliar code, reading a diff, recalling known issues with a library version | Made-up changelogs, issue numbers, and config flags. Verify every citation. |
| Hypothesize | Brainstorming competing hypotheses you hadn't considered | A single confident answer. Ask for five ranked hypotheses and what would disprove each. |
| Predict | Suggesting what each hypothesis implies you'd measure | Predictions that can't fail |
| Experiment | Writing repro scripts, load tests, queries, and instrumentation | Running generated scripts against production without reading them first |
| Analyze | Comparing before/after data, spotting patterns | Being talked into "supported" on thin data. Ask it to argue the other side. |
| Conclude | Drafting the write-up and the regression test | A fix that treats the symptom, like adding a retry or raising a timeout |

**The rule:** anything an AI tells you is a *hypothesis*. It becomes a conclusion only after an experiment you designed produces evidence.

**Data handling:** don't paste secrets, credentials, customer data, or regulated data into AI tools that aren't approved for it. Redact logs first.

## Domain playbooks

Each playbook uses the same structure: typical symptoms, observation tooling, common hypotheses with the experiment that tells them apart, a worked example, domain-specific experiment techniques, AI prompts that help, and links to the theory in this repo's curricula.

| Playbook | Covers |
|---|---|
| [Frontend](frontend.md) | Core Web Vitals, rendering, bundle size, state bugs, memory leaks, accessibility, cross-browser |
| [Backend](backend.md) | Latency, error spikes, concurrency, memory and CPU, dependency failures |
| [Database](database.md) | Query plans, indexing, locks and deadlocks, connection pools, replication lag, data integrity |
| [Quality Assurance](quality-assurance.md) | Flaky tests, escaped defects, weak tests, environment-dependent failures |
| [DevOps](devops.md) | CI failures, slow pipelines, deploy regressions, config drift |
| [Cloud](cloud.md) | Quotas and throttling, IAM denials, cost spikes, managed-service behavior, regional problems |
| [Infrastructure](infrastructure.md) | Hosts, containers and Kubernetes, networking, DNS, TLS, disk, capacity |
| [Security](security.md) | Vulnerability triage, suspicious activity, verifying controls, supply chain |

Problems rarely stay in one layer. A "slow page" can be a frontend bundle, a backend N+1 query, a missing database index, or a saturated node. Start in the layer where the symptom shows, and follow the evidence across playbooks.

## In the mentoring model

- **Juniors** learn the loop from their Mid-level mentor on real tickets in the current codebases, with AI as the lab assistant. See [mid-to-junior.md](../mentoring/mid-to-junior.md).
- **Mid-levels** learn the theory that makes hypotheses good from their Senior mentor: *why* a missing index causes a sequential scan, *why* a lock causes a convoy. See [senior-to-mid.md](../mentoring/senior-to-mid.md).
- **Seniors** review experiment logs, not just fixes, and coach on hypothesis quality.
