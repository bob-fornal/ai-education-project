# Program Operations

[Back to mentoring](README.md)

How to set up, run, and measure the tiered mentoring model. This is written for engineering managers, team leads, and whoever owns the program.

## Roles

| Role | Responsibilities |
|---|---|
| **Program owner** (usually an engineering manager or staff engineer) | Forms pods, protects time, runs quarterly check-ins, tracks health metrics, resolves escalations |
| **Senior mentor** | Runs the [Senior → Mid track](senior-to-mid.md) for their pod. Reviews Mid-levels' experiment logs. Backs up Mid-level mentors when they're stuck. |
| **Mid-level mentor** | Takes part in the Senior → Mid track as a learner. Runs the [Mid → Junior track](mid-to-junior.md) for 1–2 Juniors. |
| **Junior** | Works through the Mid → Junior track and the Junior learning path. Keeps experiment logs. Gives honest feedback on mentoring. |
| **Line manager** | Makes sure mentoring time appears in sprint planning and performance reviews. Watches workload. |

## Pod structure

A **pod** is the basic unit: one Senior, 2–3 Mid-levels, and each Mid-level's 1–2 Juniors.

```text
                    Senior
          ┌───────────┼───────────┐
          ▼           ▼           ▼
        Mid A       Mid B       Mid C
       ┌──┴──┐        │        ┌──┴──┐
       ▼     ▼        ▼        ▼     ▼
      J1    J2       J3       J4    J5
```

Guidelines:

- **Keep pods on the same team or product area** when possible, so Mid → Junior mentoring happens in a shared codebase.
- **Senior-to-Mid pods can cross teams.** Computer science content doesn't depend on the codebase, and cross-team pods spread knowledge.
- **Cap each Senior at 4 Mid-levels and each Mid-level at 2 Juniors.** Past that, quality drops quickly.
- **Mid-levels new to the role** start with one Junior.
- **If there aren't enough Mid-levels,** a Senior can mentor a Junior directly for a while, but using the Mid → Junior track content, not the old codebase-tour model.

## Time allocation

| Role | Mentoring others | Being mentored / studying | Notes |
|---|---|---|---|
| Senior | ~2–3 hrs/week | n/a | Weekly pod session, 1:1s, log reviews |
| Mid-level | ~2–4 hrs/week | ~2–3 hrs/week | Plan sprint capacity for both. This is the group most at risk of overload. |
| Junior | n/a | ~4–6 hrs/week in stages 1–3, ~2 hrs later | Much of it happens while doing real tickets |

Put this time in sprint planning as capacity, not as a hope. Mentoring that has to fit around full sprint commitments is the first thing dropped.

## Cadence

| Frequency | Senior → Mid | Mid → Junior | Program owner |
|---|---|---|---|
| Daily | | Short check-in (stage 2) | |
| Weekly | Pod session (60 min) | Session (30–60 min); PR walk-throughs | |
| Biweekly | 1:1 with each Mid (30 min) | Experiment log review | |
| Monthly | Open design review | Guided bug hunt | Pulse survey (3 questions) |
| Quarterly | Milestone check | Stage check | Pod health review; adjust pairings |

## Rollout

### Phase 1: Pilot (one quarter)

1. Pick one or two pods with willing volunteers at every level.
2. Brief everyone on the model using the [mentoring overview](README.md).
3. Run the first quarter of the [Senior → Mid track](senior-to-mid.md#q1-foundations-of-reasoning) and stages 1–3 of the [Mid → Junior track](mid-to-junior.md#stages).
4. Collect feedback every month. Expect to adjust session length and cadence.

### Phase 2: Transition existing pairings

Most teams already have Senior → Junior pairings. Don't cut them off abruptly.

1. For 4–6 weeks, the Senior, the Junior, and the incoming Mid-level mentor overlap. The Senior introduces the Mid-level and hands over context about the Junior's progress.
2. The Mid-level takes over day-to-day mentoring. The Senior keeps a monthly skip-level check-in with the Junior for one quarter.
3. The Senior forms or joins a Senior → Mid pod with the time that frees up.

### Phase 3: Expand

Roll out team by team using what the pilot learned. Keep the pilot pods' Seniors and Mid-levels available as advisors to new pods.

## Measuring it

Measure to find problems, not to rank people. Any metric that becomes a target gets gamed.

| Area | Signals worth watching | Where to find it |
|---|---|---|
| Junior progress | Time to first merged PR; review rework rate trending down; share of AI mistakes the Junior catches before review; stage progression | PR history, mentor notes |
| Quality of AI-assisted work | Escaped defects in AI-assisted changes; security findings in review; test quality (mutation score where used) | Incident and defect tracking, SAST/SCA results |
| Mid-level growth | Talks given; design reviews led; experiment log quality over time; promotion readiness | Senior notes, milestone records |
| Diagnostic practice | Number and quality of experiment logs; time to root cause on incidents | Log repository, incident reviews |
| Program health | Sessions held vs. planned; pulse survey results; mentor workload; retention of Juniors and Mid-levels | Program owner tracking |

**Monthly pulse survey** (kept short so people answer it):

1. How useful was mentoring this month? (1–5)
2. Did your sessions happen as planned? (Yes / Mostly / No)
3. What's one thing that would make it better? (free text)

## Recognition

Mentoring must count, or it won't survive the first deadline.

- Put mentoring in the career ladder at Mid-level and above. For Mid-levels, successful Junior mentoring is evidence for Senior promotion.
- Mention mentoring outcomes in performance reviews, with concrete evidence: Juniors' progress, talks given, logs reviewed.
- Share wins in public: a Junior's first solo incident diagnosis, a Mid-level's first design review.

## Failure modes

| Failure mode | Early warning | Mitigation |
|---|---|---|
| **Mid-level overload** | Mid-levels skip their own Senior sessions to help Juniors, or their delivery drops | Enforce the 2-Junior cap. Count mentoring as sprint capacity. The Senior watches for it. |
| **Sessions get cancelled for delivery** | Sessions held vs. planned falls below ~75% | Program owner escalates. Protect the time at the manager level. |
| **Juniors get lost in codebase questions** | Juniors say they don't understand the system at a high level | Mid-levels add verified codebase tours. The team invests in written architecture docs. Seniors hold a quarterly "system overview" session for all Juniors. |
| **The Senior track turns into ticket help** | Sessions cover tickets, not curriculum | Separate office hours for ticket questions. Protect the pod session. |
| **"AI does the teaching"** | Juniors can't explain their own PRs | Enforce the PR walk-through. Add "explain without AI" checks. |
| **Knowledge silos** | Only one Mid-level understands a subsystem | Rotate Juniors across Mid-levels on the same team once a year |
| **Seniors disconnect from Juniors** | Seniors don't know Juniors by name | Keep Seniors in code review and hold a quarterly skip-level session |

## Templates

### Session note (keep it short)

```markdown
**Date:** YYYY-MM-DD   **Track:** Senior→Mid | Mid→Junior   **Format:** <format>
**Attendees:** <names>
**Topic / material:** <talk, topic, ticket, or log>
**Key points:** 
- 
**Gaps found:** <things to revisit>
**Next actions:** <owner — action — due>
```

### Quarterly check-in questions

For the learner:
- What can you do now that you couldn't do last quarter? Show an example.
- Where did AI help most, and where did it mislead you?
- What do you want to focus on next quarter?

For the mentor:
- What evidence shows progress? (PRs, logs, talks, designs)
- What's getting in the way?
- Is the pairing working? Should anything change?

## FAQ

**Doesn't this take Seniors away from Juniors entirely?**
No. Seniors still review Juniors' code, lead incidents Juniors take part in, and hold skip-level sessions. They're just not the primary day-to-day mentor.

**What if a Mid-level doesn't want to mentor?**
Start with volunteers. Not everyone is ready at the same time. But over the long run, mentoring is part of growing toward Senior, and the career ladder should reflect that.

**What if the Mid-level doesn't know the answer?**
Good. That's a chance to model the Scientific Method: write a hypothesis, test it together, and escalate to the Senior if needed. Nobody knows everything, and Juniors benefit from seeing how a professional handles not knowing.

**How does this work on a very small team?**
With one Senior and one or two others, run both tracks inside the same weekly session: part CS content for the Mid-level, part AI-workflow coaching for the Junior, with the Mid-level leading the second half.
