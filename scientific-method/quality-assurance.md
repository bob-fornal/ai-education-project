# Quality Assurance Diagnostics

[Back to the Scientific Method](README.md)

QA diagnostics turns the method on the test suite itself. A test is already a small experiment: it predicts an outcome and checks it. When tests are flaky, slow, or miss real defects, the experiment is broken, and the team stops trusting the results. With AI generating more code and more tests, this layer matters more than it used to. Generated tests often pass for the wrong reasons.

## Typical symptoms

- A test passes and fails on the same commit (flaky)
- Tests pass locally but fail in CI, or the other way around
- A bug reached production even though "it was covered"
- High coverage numbers, yet defects keep escaping
- The suite is too slow, so people skip it
- A test only fails when run with other tests, or in a certain order

## Observation tooling

| Need | Tools |
|---|---|
| Repeating a test to measure flakiness | `pytest --count` (pytest-repeat), `pytest-randomly`, Jest `--runInBand` with a loop, Playwright `--repeat-each`, Maven Surefire `rerunFailingTestsCount`, Gradle test retry plugin (for *measuring*, not hiding) |
| Seeing what happened in a failed UI test | Playwright **trace viewer**, Cypress screenshots and video, test runner HTML reports |
| Test history and flake rates | CI test analytics (GitHub Actions test reports, GitLab test reports, Buildkite Test Analytics, Datadog CI Visibility, CircleCI Insights), JUnit XML trend dashboards |
| Coverage | Istanbul / `nyc` / V8 coverage (JS/TS), `coverage.py`, JaCoCo, Coverlet |
| Whether tests actually catch bugs | **Mutation testing**: Stryker (JS/TS, C#), PIT (Java), mutmut / cosmic-ray (Python) |
| Order dependence | Randomized ordering (`pytest-randomly`, Jest `--randomize`, JUnit 5 `MethodOrderer.Random`), running a single test in isolation |
| Contracts between services | Pact or other consumer-driven contract testing |
| Edge cases humans didn't think of | Property-based testing: Hypothesis (Python), fast-check (JS/TS), jqwik (Java), FsCheck (.NET) |
| Environment differences | Containers for test dependencies (Testcontainers), pinned tool versions, CI job re-run with SSH or debug logging |

## Common hypotheses and how to tell them apart

| Symptom | Candidate hypotheses | Discriminating experiment |
|---|---|---|
| Flaky test | (a) Timing: test doesn't wait for async work (b) Shared state leaks between tests (c) Order dependence (d) Real race condition in the code under test (e) External dependency (network, clock, randomness) | Run it alone 200 times. If it's still flaky, it's (a), (d), or (e). If it's clean alone but flaky in the suite, it's (b) or (c). Randomize order with a fixed seed and bisect the preceding tests. Freeze the clock and seed randomness for (e). Add a delay inside the code under test to widen a suspected race for (d). |
| Passes locally, fails in CI | (a) Different tool or runtime version (b) Timezone, locale, or path differences (c) Fewer CPUs in CI exposing a timing problem (d) Missing environment variable or secret (e) Test data left over locally | Print versions and env in both places and diff them. Run locally in the CI container image. Constrain local CPU (`docker run --cpus=1`). Run locally from a clean clone. |
| Bug escaped despite "coverage" | (a) Line covered but outcome never asserted (b) Tests mock the part that broke (c) The input that triggers it was never tried (d) Integration seam not tested | Run mutation testing on the module. Surviving mutants show where assertions are missing (a). Check what the tests mocked (b). Write a property-based test around the failing input class (c). |
| Suite too slow | (a) A few very slow tests dominate (b) Repeated expensive setup (c) Real I/O where a fake would do (d) No parallelism | Sort test durations from JUnit XML. Profile setup and teardown. Count database and network calls per test. |

## Worked example: an "intermittent" checkout test

1. **Observe.** `checkout_applies_discount` failed 14 times in the last 200 CI runs. It has never failed on a developer's machine. When it fails, the total is off by exactly the discount amount.
2. **Question.** Why does this test fail about 7% of the time in CI, always by exactly the discount amount?
3. **Research.** CI runs tests in parallel across 4 workers against a shared Postgres container. Locally, people run the one file. The discount comes from a `promotions` table seeded by a fixture.
4. **Hypothesize.**
   - H1: Another test deletes or modifies the promotion row in the shared database mid-run.
   - H2: The test doesn't wait for an async price recalculation.
   - H3: The promotion has an expiry, and CI's UTC clock sometimes crosses it.
5. **Predict.** H1: failures only happen when the suite runs in parallel, and running this test alone 200 times never fails. H2: running it alone 200 times with CPU constrained still fails sometimes. H3: failures cluster at a particular time of day.
6. **Experiment.** Run the test alone 200 times in the CI image with `--cpus=1`. Separately, run the full suite 300 times with 4 workers, logging every write to `promotions` with the test name and timestamp.
7. **Analyze.** Alone, it passed 200 of 200 even with CPU constrained, so H2 is refuted. The historical failures are spread across the day, so H3 is refuted. The parallel runs produced 21 failures, and in every one the write log shows `admin_can_delete_promotion` truncating `promotions` on another worker while the checkout test was mid-run. H1 is supported.
8. **Conclude.** Each test now runs inside a transaction that rolls back, and `admin_can_delete_promotion` creates its own row instead of truncating. 500 parallel runs, zero failures. Randomized test ordering is now on by default so order-dependent tests show up sooner.

## Experiment techniques

- **Quantify flakiness before and after.** "Seems better" isn't a result. "14 of 200 failed before, 0 of 500 after" is.
- **Retries are for measurement, not for hiding.** Auto-retrying a flaky test in CI keeps the pipeline green while the underlying race ships to production. Quarantine flaky tests visibly and track them.
- **Mutation testing checks the tests.** If you flip `>` to `>=` in the code and every test still passes, the tests don't check that boundary.
- **Reproduce the CI environment locally.** Run the CI container image, with the same CPU and memory limits and the same env vars.
- **Keep a failing test first.** When a defect escapes, write the test that fails *before* writing the fix. That test is your reproduction and your regression guard.

## Reviewing AI-generated tests

AI writes tests quickly, and it writes a lot of weak ones. Before accepting generated tests, check:

- [ ] Does each test assert on a *behavior*, not just that a function was called?
- [ ] Would the test fail if the code were wrong? Break the code on purpose and check.
- [ ] Are the mocks hiding the very thing that could break?
- [ ] Are edge cases (empty, null, boundary values, unicode, time zones) covered, or only the happy path?
- [ ] Does the test depend on wall-clock time, randomness, or ordering?
- [ ] Is the expected value independently derived, or was it copied from what the code currently outputs?

## AI prompts that help

- "Here's a flaky test and the three most recent failure logs. List hypotheses for the flakiness, ranked, and tell me the cheapest experiment to separate them."
- "Here are the surviving mutants from Stryker for this module. Write tests that would kill each one, and explain what behavior each test pins down."
- "Write a property-based test with fast-check for this discount function. The invariants are: the total is never negative, and the discount never exceeds the subtotal."

**Pitfall:** Ask an AI to "fix the flaky test" and it will usually add a sleep, a retry, or a longer timeout. That hides the symptom and leaves the cause alone.

## Theory behind the playbook

- [CS14 Software Engineering](../computer-science-in-ai-curriculum/talks/14-software-engineering.md): testing strategy, review, and process discipline
- [CS07 Introduction to Computer Systems](../computer-science-in-ai-curriculum/talks/07-introduction-to-computer-systems.md): why concurrency bugs are intermittent
- [CS04 Discrete Mathematics for CS](../computer-science-in-ai-curriculum/talks/04-discrete-mathematics-for-cs.md): invariants and reasoning behind property-based tests
- [SDA1 Clean Code Principles](../computer-science-software-design-and-architecture/curriculum/part-1/part-1-1--clean-code-principles/README.md): fast, independent tests
- [SDA3 Functional Programming](../computer-science-software-design-and-architecture/curriculum/part-1/part-1-3--functional-programming/README.md): pure functions are the easiest code to test
