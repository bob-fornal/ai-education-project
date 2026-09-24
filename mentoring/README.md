# Tiered Mentoring

[Back to the project README](../README.md)

## The shift

For a long time, mentoring on most teams looked the same. A Senior developer sat with a Junior and walked them through the existing codebase: where things live, why that module is weird, how to run the tests, what the deploy process expects. The Senior was the map.

That model is wearing out, for three reasons.

1. **AI now answers a lot of the "map" questions.** A Junior can ask an AI assistant to explain a module, trace a request, or find where a setting is read. It's not always right, but it's there at 2 a.m. and it never gets impatient. Spending a Senior's scarce hours on codebase orientation doesn't pay off the way it used to.
2. **Juniors need a different skill now.** The hard part for a Junior isn't finding the code anymore. It's using AI as a developer without being fooled by it: specifying work clearly, reading generated code critically, testing it, and knowing when to stop trusting it. The people best placed to teach that are the ones doing it every day in the current codebases, and that's usually the Mid-levels.
3. **Mid-levels are at risk of plateauing.** A Mid-level who's fluent with AI tools can ship a lot. Without deeper computer science and design knowledge, they become an "AI operator" who can't tell when confident output is wrong. The [priority test](../_DOCS/cs-in-ai-curriculum.md#the-priority-test) in the CS in AI curriculum names this exactly: weakness in fundamentals becomes *dangerous* when AI is doing the typing. Seniors are the right people to close that gap.

## The tiered model

```text
┌──────────────────────────────────────────────────────────────────────┐
│ SENIORS                                                              │
│ Teach: computer science depth, architecture, diagnostic reasoning    │
│ Draw on: CS in AI talks, Software Design & Architecture topics       │
└───────────────────────────────┬──────────────────────────────────────┘
                                │ Senior → Mid-level track
                                ▼
┌──────────────────────────────────────────────────────────────────────┐
│ MID-LEVELS                                                           │
│ Learn: why systems behave the way they do                            │
│ Teach: using AI as a developer in the current codebases              │
└───────────────────────────────┬──────────────────────────────────────┘
                                │ Mid-level → Junior track
                                ▼
┌──────────────────────────────────────────────────────────────────────┐
│ JUNIORS                                                              │
│ Learn: AI-assisted development, code review, testing, and the        │
│        Scientific Method, on real tickets in the real codebases      │
└──────────────────────────────────────────────────────────────────────┘

            Shared language across every tier: the Scientific Method
```

| | Senior → Mid-level | Mid-level → Junior |
|---|---|---|
| **Focus** | Computer science and architecture | AI-assisted development in the current codebases |
| **Core question** | "Why does the system behave this way, and how should it be designed?" | "How do I get correct, safe, maintainable work done with AI here?" |
| **Content** | [CS in AI talks](../computer-science-in-ai-curriculum/README.md), [Software Design & Architecture topics](../computer-science-software-design-and-architecture/README.md), diagnostic theory | Prompting and specification, reviewing and testing AI output, team conventions, security hygiene, the [Scientific Method playbooks](../scientific-method/README.md) |
| **Typical formats** | Talk-and-discuss, design reviews, "explain it back," experiment log reviews | Pair programming with AI, prompt retros, PR walk-throughs, guided bug hunts |
| **Outcome** | Mid-levels who can catch what AI gets wrong and are on the path to Senior | Juniors who ship safely with AI and can explain every line they merge |
| **Details** | [senior-to-mid.md](senior-to-mid.md) | [mid-to-junior.md](mid-to-junior.md) |

## Old model vs. tiered model

| | Old: Senior → Junior on the codebase | New: tiered |
|---|---|---|
| Senior time goes to | Codebase orientation, answering "where is X" | Deep fundamentals and design judgment, which AI can't substitute for |
| Junior learns from | Someone several career stages away | Someone who was a Junior recently and uses the same AI workflow daily |
| Mid-levels | Mostly left out of mentoring, on either side | Both learners and teachers. Teaching cements what they know. |
| Codebase knowledge flows through | One Senior's memory | Mid-levels, AI assistants, and written docs together |
| Main risk addressed | Juniors getting lost in the code | Juniors trusting bad AI output, and Mid-levels plateauing |
| Scales with | Number of Seniors (the scarcest group) | Number of Mid-levels (usually the largest group) |

## Principles

1. **Each tier teaches what it's closest to.** Seniors are closest to fundamentals and long-horizon design. Mid-levels are closest to the day-to-day AI workflow in today's code.
2. **Teaching is part of learning.** Mid-levels deepen their own understanding by explaining it. Expect them to find gaps in what they know while teaching, and plan for that.
3. **The Scientific Method is the shared language.** Every tier writes [experiment logs](../scientific-method/experiment-log-template.md) the same way. Seniors review Mid-levels' logs. Mid-levels review Juniors'.
4. **AI is a tool in every tier, not a teacher in any tier.** Mentors use AI in sessions openly. They also check that learners can do the reasoning without it.
5. **Seniors don't disappear from Juniors' lives.** Seniors still review code, lead incidents, and hold occasional skip-level sessions. They just aren't the primary mentor.
6. **Mentoring is real work.** It's scheduled, counted in performance reviews, and protected from sprint pressure. See [program operations](program-operations.md).

## What stays the same

- Seniors still do code review across all levels, and still own architecture decisions.
- Any Junior can escalate to a Senior when a Mid-level mentor is stuck or unavailable.
- Onboarding basics (accounts, environments, team norms) are still handled by the team and manager, not left to mentors.

## Documents in this folder

| Document | For |
|---|---|
| [senior-to-mid.md](senior-to-mid.md) | Seniors and Mid-levels: the computer science track, session formats, quarterly milestones |
| [mid-to-junior.md](mid-to-junior.md) | Mid-levels and Juniors: the AI-as-a-developer track, stages, review checklists |
| [program-operations.md](program-operations.md) | Leads and managers: pod structure, cadence, rollout, measurement, and failure modes |
