# Software Engineering

**Backbone Course #14** · **Duration:** 50 minutes

## The One-Sentence Pitch
Writing code that works once is programming; making sure it keeps working correctly for years, across a team, as requirements change, is software engineering — and that shift needs process, not just skill.

## Audience & Prerequisites
This talk is for learners who have written individual programs or scripts but have not yet worked on a multi-person, long-lived codebase; no prior process or tooling experience is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: a script versus a system |
| 0:05–0:15 | The software lifecycle and why process matters |
| 0:15–0:25 | Requirements and design before code |
| 0:25–0:36 | Testing strategies |
| 0:36–0:46 | Version control and collaboration workflows |
| 0:46–0:50 | Key takeaway and close |

### Hook: a script versus a system
Open with a contrast: a 50-line script you write for yourself to rename some files is fine to hack together with no plan, because if it's wrong, you notice immediately and you're the only one affected. Now imagine that same codebase has grown into a system used by a million people, maintained by fifty engineers who weren't there when it was written, updated weekly, for the next five years. Everything that was optional for the script — documentation, tests, a deliberate process for making changes — becomes mandatory for the system, not because the code is inherently different, but because the *cost of a mistake* and the *number of people who need to understand it* have both exploded. Software engineering is the set of practices that exist specifically to manage that explosion.

### The software lifecycle and why process matters
The software lifecycle names the recurring stages a real system moves through — typically requirements gathering, design, implementation, testing, deployment, and maintenance — and the crucial thing to notice is that maintenance usually dwarfs the others in total time and cost, because a successful system gets used, changed, and patched for years after it first ships. Process — code review, planning meetings, defined stages before code merges — feels like overhead on a small project, but its actual purpose is to catch problems while they're cheap: a misunderstood requirement caught in a planning conversation costs an hour, the same misunderstanding caught after six months of building on top of it can cost weeks of rework. The lone-script scenario from the hook can skip nearly all of this because the "team" is one person for one afternoon; the moment a codebase must outlive its author's memory or involve more than one person, skipping process doesn't remove the cost, it just defers it and makes it larger.

### Requirements and design before code
Requirements are a precise statement of what a system actually needs to do — not what the first person to describe it assumed, but what's genuinely needed — and gathering them well means asking clarifying questions before writing a line of code, because an ambiguous requirement doesn't disappear when you start coding, it just becomes an ambiguous piece of code. Design is the step of deciding the shape of a solution — which components exist, how they communicate, where data lives — before committing to implementation details, the software equivalent of sketching a floor plan before pouring concrete. The reason "just start coding" fails past a certain size is that early structural decisions become extremely expensive to reverse once dozens of other pieces depend on them — realizing you need a completely different data model after a system is half-built means rewriting the half that's already done, whereas catching that same issue on a whiteboard costs an afternoon.

### Testing strategies
Unit tests check one small piece of code in isolation — a single function, given specific inputs, produces the expected output — and they're valuable because when one fails, you know almost exactly where the bug is, since everything else was held constant. Integration tests instead check that multiple pieces work correctly *together* — does the part that reads from the database actually hand off correctly to the part that formats a response — and they catch a different, equally real class of bug: two components that are each individually correct but make incompatible assumptions about each other. Neither replaces the other: unit tests could all pass while an integration bug (a mismatched data format between two teams' components) breaks the system entirely, and conversely integration tests alone would make it painfully slow to pinpoint which of dozens of components caused a failure. A healthy test suite deliberately has many fast, narrow unit tests and fewer, more expensive integration tests, because that balance catches the most bugs per minute of testing time.

### Version control and collaboration workflows
Version control (most commonly Git) tracks every change to a codebase over time, recording who changed what, when, and why — which sounds like bookkeeping until you need to find out exactly when and how a bug was introduced, at which point that history is the difference between a five-minute investigation and a multi-day one. Branches let multiple people work on different changes simultaneously without stepping on each other's in-progress work, merging their changes back together only once each piece is ready. Code review — having another engineer read and approve a change before it merges — exists because a second pair of eyes reliably catches mistakes the original author is too close to see, and it also spreads knowledge of the codebase across the team instead of trapping it in one person's head. All of this is the connective tissue that makes "fifty engineers changing the same system" actually work, rather than devolving into constant conflicting, undocumented changes.

### Key takeaway and close
Return to the hook's script-versus-system framing: every practice covered today — lifecycle awareness, requirements and design, layered testing, version control and review — is a direct response to the same underlying problem, that mistakes and misunderstandings get dramatically more expensive as scale, team size, and lifespan increase. None of these practices make sense for a one-off script, and all of them become close to mandatory the moment a codebase needs to survive its author and its first version.

## Key Takeaway
Software engineering practices exist to control the cost of mistakes and miscommunication as a codebase grows in size, team, and lifespan — process isn't bureaucracy for its own sake, it's how a system stays correct and maintainable at a scale no individual can hold in their head.

## Go Deeper
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.005 — Software Construction](https://ocw.mit.edu/courses/6-005-software-construction-spring-2016/pages/syllabus/)
- Harvard PLL: [CS50's Web Programming with Python and JavaScript](https://pll.harvard.edu/course/cs50s-web-programming-python-and-javascript)
