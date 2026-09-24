# Experiment Log Template

[Back to the Scientific Method](README.md)

Copy this into the ticket, the incident doc, or a `diagnostics/` folder in the affected repo. Fill it in *while* you work, not afterward. A log with three refuted hypotheses is more useful than a one-line "fixed it."

---

```markdown
# Investigation: <short symptom description>

- **Ticket / incident:** <link>
- **Investigator(s):** <names>
- **Mentor / reviewer:** <name, if part of mentoring>
- **Domain(s):** Frontend | Backend | Database | QA | DevOps | Cloud | Infrastructure | Security
- **Started:** <YYYY-MM-DD HH:MM TZ>   **Closed:** <YYYY-MM-DD HH:MM TZ>

## 1. Observations
Facts only, each with a source. No interpretation yet.
- <timestamp> — <what was seen> — <source: dashboard link, log query, user report>

## 2. Question
One precise question: what, where, since when, for whom, how often.
> <question>

## 3. Research
- **Baseline / normal:** <metric values when healthy, with source>
- **Recent changes:** <deploys, config, flags, dependency bumps, traffic, data volume>
- **Known issues checked:** <links>

## 4. Hypotheses
| # | Hypothesis (mechanism) | Likelihood | Cost to test | Status |
|---|---|---|---|---|
| H1 | | High/Med/Low | Low/Med/High | Open / Supported / Refuted / Inconclusive |
| H2 | | | | |
| H3 | | | | |

## 5–7. Experiments
Repeat this block per experiment.

### Experiment <n> — testing H<#>
- **Prediction if true:** <measurable outcome>
- **Prediction if false:** <measurable outcome>
- **Variable changed:** <exactly one>
- **Control:** <what stayed the same / comparison group>
- **Environment:** local | test | staging | canary | production (read-only?)
- **Method:** <commands, scripts, queries — paste or link them>
- **Results:** <raw data, screenshots, numbers>
- **Analysis:** <supported / refuted / inconclusive, and why; alternative explanations considered>

## 8. Conclusion
- **Root cause:** <mechanism, with the evidence that proves it>
- **Fix:** <PR link>
- **Verification:** <how the fix was proven — same experiment, now passing>
- **Recurrence guard:** <regression test, alert, lint rule, runbook entry>
- **What would have found this sooner:** <missing log, metric, test, or knowledge>

## AI usage
- **Tools used:** <tool/model>
- **What it contributed:** <hypotheses, scripts, summaries>
- **What it got wrong:** <useful for calibrating trust next time>
- **Data shared:** <confirm no secrets / PII / regulated data>
```

---

## Review checklist

Mentors and reviewers can use this to coach, not grade.

- [ ] Observations are facts with sources, not conclusions
- [ ] The question is specific enough that you'd know when it's answered
- [ ] There's a baseline to compare against
- [ ] At least two competing hypotheses were written down
- [ ] Each experiment changed one variable and had a control
- [ ] Predictions were written *before* results
- [ ] The root cause names a mechanism, not just a location ("the cache" is a location; "entries never evict because TTL is read in ms but set in s" is a mechanism)
- [ ] The fix was verified by re-running the experiment
- [ ] Something now exists to catch this sooner next time
- [ ] AI contributions were checked, and no sensitive data was shared
