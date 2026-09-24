
# Human-Computer Interaction

**Backbone Course #23** · **Duration:** 50 minutes

## The One-Sentence Pitch
Good software design starts from the user's goals and mental model rather than the engineer's implementation, gets tested cheaply before it gets polished, and treats accessibility as a starting requirement rather than a fix applied afterward.

## Audience & Prerequisites
For learners with any prior exposure to building or using software interfaces; no design background is assumed, and no math beyond simple arithmetic is used.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Framing: design for the user, not the implementation |
| 0:05–0:17 | Usability heuristics and user-centered design |
| 0:17–0:30 | The design-prototype-evaluate loop |
| 0:30–0:40 | Cognitive load and Fitts's-law-style reasoning |
| 0:40–0:47 | Accessibility as a first-class requirement |
| 0:47–0:50 | Synthesis and close |

### Framing: design for the user, not the implementation
It's natural for the person who built a system to design its interface around how the system actually works internally — which buttons map cleanly to which functions, which screens correspond to which database tables. Human-computer interaction (HCI) is built on the observation that this instinct is usually wrong: users don't care how the system is implemented, they care about accomplishing their own goal, and they bring their own existing mental model of how the task "should" work, often shaped by other tools they've used before. A well-designed interface meets the user's mental model instead of forcing the user to learn the engineer's internal model of the system. This single reframing — starting design from the user's goals rather than the implementation's structure — underlies essentially everything else in this talk.

### Usability heuristics and user-centered design
User-centered design is a process discipline: before writing any interface, you study who the actual users are, what they're trying to accomplish, and what confuses or blocks them today, and you keep checking your design against that understanding throughout the process rather than just once at the start. This is often supported by usability heuristics — general, well-tested rules of thumb for evaluating an interface, such as keeping the system's current state visible to the user at all times, using familiar, real-world language and concepts rather than system-internal jargon, and giving users an obvious way to undo a mistake or exit an unwanted state. These heuristics aren't rigid laws; they're a fast, practical checklist for catching common usability problems before you've spent real engineering effort building the wrong thing.

### The design-prototype-evaluate loop
A core HCI practice is to never jump straight from an idea to a finished, polished product. Instead, teams build a prototype — a rough, cheap stand-in for the eventual interface — evaluate it with real or representative users, learn what's confusing or missing, and revise, repeating this loop multiple times before investing in a fully built, polished version. Prototypes deliberately start low-fidelity: a paper sketch or a simple clickable mockup can reveal that users don't understand a proposed flow at all, at a cost of an hour of work, instead of discovering the same problem after weeks of real engineering. Low-fidelity prototypes also invite more honest feedback — a rough sketch visibly invites criticism and change, while a polished, finished-looking screen can make people hesitant to suggest fundamental changes even when the design has real problems. The core principle is: find out what's wrong as cheaply as possible, as early as possible.

### Cognitive load and Fitts's-law-style reasoning
Beyond flow and structure, HCI also cares about the physical and mental effort an interface demands, which is called cognitive load — how much a user has to remember, track, or think about just to complete a task, separate from the difficulty of the task's underlying goal. High cognitive load shows up as things like too many simultaneous choices, inconsistent placement of controls, or requiring the user to hold information in their head across multiple screens. One of the most concrete, memorable results in this space is Fitts's-law-style reasoning: the time it takes a user to accurately hit a target with a pointer (a cursor, a finger) depends predictably on the target's size and its distance from the starting point — bigger targets, and targets closer to where the user's attention already is, are measurably faster and more reliable to hit. This is why a frequently-used button is made large and placed somewhere natural for the hand or eye to be already positioned, while a rarely-used, dangerous action (like "delete account") is often made deliberately small or tucked away — the same underlying human-factors reasoning, applied in opposite directions on purpose.

### Accessibility as a first-class requirement
Accessibility means designing so people with disabilities — visual, motor, auditory, cognitive — can use an interface as effectively as anyone else, and the central HCI lesson here is that it has to be a starting requirement, not a retrofit applied at the end. Concretely, this means structuring content so a screen reader can correctly announce it to a blind or low-vision user, ensuring interactive elements are large and spaced enough to be usable by someone with limited fine motor control, and never relying on color alone to convey information, since a meaningful fraction of users have some form of color blindness. Retrofitting accessibility after a design is finalized is consistently harder and more expensive than designing for it from the start, precisely because so many core structural decisions — layout, navigation order, how information is grouped — are difficult to change later without redoing much of the interface; building accessibility in from the first prototype avoids that expensive rework entirely.

### Synthesis and close
All four ideas connect: user-centered design tells you whose goals to design for, the prototype-evaluate loop tells you how to find out cheaply whether you're meeting those goals, Fitts's-law-style reasoning gives you one concrete, measurable lever for reducing physical effort, and accessibility ensures the result actually works for the full range of real users rather than an implicit "default" user. None of these is a one-time checklist item — they're a way of continually asking "what is this actually like for the person using it?" throughout a design process.

## Key Takeaway
Design for the user's goals and mental model, test cheap prototypes before building polished ones, and treat accessibility as a requirement from the very first sketch — retrofitting any of these later is reliably more expensive than starting with them.

## Go Deeper
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.813 — User Interface Design and Implementation](https://ocw.mit.edu/courses/6-831-user-interface-design-and-implementation-spring-2011/pages/syllabus/)
