# DevOps Diagnostics

[Back to the Scientific Method](README.md)

DevOps problems live in the path from a commit to running software: builds, pipelines, artifacts, configuration, and deploys. These systems are full of hidden state (caches, runner images, floating dependency versions, environment variables), which is why "nothing changed" is so often false. The first job is usually finding what actually changed.

## Typical symptoms

- The build broke, and nobody touched the failing code
- A pipeline got much slower over a few weeks
- A deploy succeeded, but the service misbehaves afterward
- Staging works and production doesn't
- A rollback didn't fix the problem
- Environments slowly diverge from what's in version control (drift)
- Deploy frequency or lead time is getting worse (DORA metrics)

## Observation tooling

| Need | Tools |
|---|---|
| Pipeline logs and timing | Your CI system's logs and step timings (GitHub Actions, GitLab CI, Azure Pipelines, Jenkins, CircleCI, Buildkite). Turn on debug logging (GitHub Actions: `ACTIONS_STEP_DEBUG` / `ACTIONS_RUNNER_DEBUG` secrets or "re-run with debug logging"). |
| Running a pipeline locally | `act` (GitHub Actions), `gitlab-runner exec` (older versions), or running the same container image and script by hand |
| What changed between two builds | `git diff`, lockfile diffs (`package-lock.json`, `poetry.lock`, `go.sum`), base image digests, runner image version notes |
| Which commit broke it | `git bisect run ./ci-repro.sh` |
| Dependency drift | Lockfiles, `npm ls`, `pip freeze`, Renovate or Dependabot history, SBOM diffs (Syft) |
| Artifact identity | Image digests (not tags), `docker inspect`, provenance and signatures (Sigstore / cosign, SLSA attestations) |
| Deploy state vs. desired state | `terraform plan`, `pulumi preview`, Argo CD / Flux diff and sync status, `helm diff`, `kubectl diff` |
| Deploy effect on the service | Deploy markers on dashboards, canary analysis (Argo Rollouts, Flagger, Spinnaker Kayenta), feature-flag audit logs |
| Delivery health over time | DORA metrics: deployment frequency, lead time for changes, change failure rate, time to restore |

## Common hypotheses and how to tell them apart

| Symptom | Candidate hypotheses | Discriminating experiment |
|---|---|---|
| Build broke, code unchanged | (a) Floating dependency version pulled a breaking release (b) CI runner image updated (c) Cache poisoned or stale (d) External service or registry outage (e) Expired credential or certificate | Diff the lockfile resolution between the last green and first red build. Compare runner image versions in the logs. Re-run with the cache disabled. Check registry status and credential expiry dates. |
| Pipeline slowed over weeks | (a) Cache no longer hits (key changed) (b) Test suite growth (c) Larger dependency tree or image (d) Runner contention or queueing | Chart step durations over time and see which step grew. Check cache hit/miss logs. Separate queue time from run time. |
| Deploy succeeded, service misbehaves | (a) Code regression (b) Config or secret differs from staging (c) Migration ran partially or out of order (d) Deploy ran a different artifact than the one tested | Compare the image digest deployed with the digest tested. Diff rendered config between environments. Check the migration table. Roll back a single canary. |
| Rollback didn't fix it | (a) The cause isn't in the code (config, data, dependency) (b) A migration isn't reversible and stayed applied (c) Caches or CDNs still serve the new version (d) The cause coincided with the deploy but wasn't caused by it | Check what the rollback actually reverted. Inspect schema state. Purge caches in a controlled way. Look at dependency health for the same window. |
| Drift | (a) Manual console changes (b) Out-of-band automation (c) Provider-side defaults changed | Run `terraform plan` (or the equivalent) and read the diff. Check cloud audit logs (CloudTrail, Azure Activity Log, GCP Audit Logs) for who changed it. |

## Worked example: "nothing changed" but the build is red

1. **Observe.** Main went red Tuesday at 09:14. The failing step is `npm test`. One snapshot test fails: `formatInvoiceTime` expected `"3:45 PM"` and received what looks like the same string, except the space before `PM` is now U+202F, a narrow no-break space. The last commit touched only README files.
2. **Question.** Why does the `formatInvoiceTime` snapshot fail on main from Tuesday 09:14 on, when no code changed?
3. **Research.** The last green run was Monday 17:40. The build job uses `actions/setup-node` pinned to Node 20. The repo has a `package-lock.json`, but CI runs `npm install`, not `npm ci`. Nobody on the team has looked at the test job's YAML in a year.
4. **Hypothesize.**
   - H1: `npm install` resolved a newer transitive dependency (a date library) that formats differently.
   - H2: The Node runtime used by the tests changed, bringing different ICU locale data.
   - H3: A stale cache restored a mismatched `node_modules`.
5. **Predict.** H1: the resolved dependency tree differs between the green and red runs, and `npm ci` against the committed lockfile passes. H2: `node --version` or `process.versions.icu` differs between the runs. H3: re-running with the cache disabled passes.
6. **Experiment.** Re-run the red commit three ways: with the cache disabled, with `npm ci`, and with a debug step that prints `node --version` and `process.versions.icu` inside the test job.
7. **Analyze.** The cache-disabled run fails, so H3 is refuted. The `npm ci` run fails, so H1 is refuted. The debug step is the surprise: the test job doesn't use the setup-node runtime at all. It runs inside `container: node:lts-alpine`, and that tag moved to a new Node major Monday night, with a newer ICU version. Running the tests in the previous image, pinned by digest, passes. H2 is supported.
8. **Conclude.** Pinned the test container by digest to the same Node version as the build job. Switched to `npm ci`. Added a pipeline check that fails if Node versions differ between jobs, and changed the snapshot to normalize whitespace since locale output isn't a contract. The log records why the first hypotheses missed: the team's mental model of the pipeline was wrong. There was a second Node runtime nobody knew about.

## Experiment techniques

- **Pin everything you can, then diff.** Pin tool versions, base images by digest, and dependencies by lockfile. Once things are pinned, "what changed" becomes answerable.
- **Rebuild the last green commit.** If the last known-good commit now fails too, the cause is outside your code: environment, dependencies, or infrastructure.
- **Bisect with a script.** `git bisect run` plus a script that reproduces the failure finds the breaking commit without guesswork.
- **Canary and compare.** Send a small slice of traffic to the new version and compare it against the old version's metrics, not against yesterday's.
- **Keep deploys and config changes separate.** When one release changes code, config, and schema at once, you can't tell which one broke things. Ship them separately when you can.

## AI prompts that help

- "Here are the logs from the last green run and the first red run of the same pipeline. Diff the environment, versions, and resolved dependencies, and list what changed."
- "Here's our workflow YAML. List every place a version floats (tags, ranges, `latest`, `lts/*`) and suggest how to pin each one."
- "Write a `git bisect run` script that returns 0 if `npm test -- dateUtils` passes and 1 if it fails, and 125 if the build itself fails."

**Pitfall:** AI often suggests clearing caches or re-running until green. That can make a failure disappear without explaining it, and it'll come back.

## Theory behind the playbook

- [CS14 Software Engineering](../computer-science-in-ai-curriculum/talks/14-software-engineering.md): build, release, and change management
- [SDA33 Operations & DevOps Knowledge](../computer-science-software-design-and-architecture/curriculum/part-8/part-8-33--operations-devops-knowledge/README.md): IaC, containers, CI/CD, service mesh
- [SDA24 Monitoring & Observability](../computer-science-software-design-and-architecture/curriculum/part-6/part-6-24--monitoring-observability/README.md): deploy markers and alerting
- [SDA27 Reliability & Resiliency Patterns](../computer-science-software-design-and-architecture/curriculum/part-7/part-7-27--reliability-resiliency-patterns/README.md): deployment stamps and health endpoints
