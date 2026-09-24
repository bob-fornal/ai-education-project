# Senior → Mid-level Track: Computer Science and Architecture

[Back to mentoring](README.md)

## Purpose

Take Mid-level developers who are already productive, often very productive with AI, and give them the depth to know *when the output is wrong and why*. By the end of a year, a Mid-level in this track should be able to:

- Explain the cost of an algorithm or data structure choice and spot a poor one in generated code
- Reason about memory, concurrency, networks, and databases well enough to form good hypotheses when things break
- Design a feature or service with explicit trade-offs, and defend them in a design review
- Understand how the AI tools they use every day actually work, and where they tend to fail
- Teach Juniors with confidence, because they understand the *why* behind the practices they teach

## Why computer science, and why Seniors

The [CS in AI curriculum](../_DOCS/cs-in-ai-curriculum.md) ranks subjects by one question: does weakness here become *dangerous* when AI writes the code? The Critical subjects (algorithms, data structures, systems, databases, security, software engineering, and how AI itself works) are exactly the ones AI can't cover for you. It produces confident answers in those areas that take real knowledge to check.

Seniors are the right mentors because they've usually learned these subjects the hard way, through production incidents and design mistakes. They can connect a textbook concept to "remember when checkout fell over." That connection is what makes theory stick.

## Structure

The track runs as four quarters, each pairing CS in AI talks with Software Design & Architecture topics. The sequence below matches the Mid-level path in [learning paths](../_DOCS/learning-paths.md).

### Q1: Foundations of reasoning

| Material | Codebase connection | Scientific Method practice |
|---|---|---|
| [CS03 Algorithms](../computer-science-in-ai-curriculum/talks/03-algorithms.md), [CS29 BFS/DFS](../computer-science-in-ai-curriculum/talks/29-graph-traversal-bfs-dfs.md), [CS30 Sorting & Searching](../computer-science-in-ai-curriculum/talks/30-sorting-and-searching-fundamentals.md) | Find one place in the codebase with accidental quadratic behavior, or prove there isn't one on the hot path | Measure it: build a benchmark, predict the growth curve, then check the prediction |
| [CS04 Discrete Math](../computer-science-in-ai-curriculum/talks/04-discrete-mathematics-for-cs.md) | Write the invariants for one core domain object | Turn the invariants into property-based tests (see the [QA playbook](../scientific-method/quality-assurance.md)) |
| [SDA5 Paradigms](../computer-science-software-design-and-architecture/curriculum/part-1/part-1-5--programming-paradigms/README.md), [SDA6](../computer-science-software-design-and-architecture/curriculum/part-2/part-2-6--core-design-principles/README.md)–[SDA8](../computer-science-software-design-and-architecture/curriculum/part-2/part-2-8--dry-yagni/README.md) Principles, SOLID, DRY/YAGNI | Run a SOLID review of one module that AI helped write | |

**Milestone:** the Mid-level presents one Q1 talk to peers from the repo outline, and answers questions without notes or AI.

### Q2: How machines run code

| Material | Codebase connection | Scientific Method practice |
|---|---|---|
| [CS07 Computer Systems](../computer-science-in-ai-curriculum/talks/07-introduction-to-computer-systems.md), [CS08 Operating Systems](../computer-science-in-ai-curriculum/talks/08-operating-systems.md) | Find where the service shares mutable state across threads or requests | Reproduce a race on purpose in a test, then fix it (see the [backend playbook](../scientific-method/backend.md)) |
| [CS09 Networks](../computer-science-in-ai-curriculum/talks/09-computer-networks.md) | Trace one request from browser to database, naming every hop | Diagnose a timeout or DNS issue with `dig`, `curl -v`, and `ss` (see the [infrastructure playbook](../scientific-method/infrastructure.md)) |
| [SDA9 Design Patterns](../computer-science-software-design-and-architecture/curriculum/part-2/part-2-9--design-patterns-gof-posa/README.md), [SDA10](../computer-science-software-design-and-architecture/curriculum/part-3/part-3-10--architectural-principles/README.md)–[SDA13](../computer-science-software-design-and-architecture/curriculum/part-3/part-3-13--enterprise-application-patterns/README.md) Architecture | Draw the codebase's real architecture, then the one it claims to have, and explain the difference | |

**Milestone:** the Mid-level writes a short architecture description of one service, reviewed by the Senior, that the team adopts as documentation.

### Q3: Systems at scale

| Material | Codebase connection | Scientific Method practice |
|---|---|---|
| [CS10 Distributed Systems](../computer-science-in-ai-curriculum/talks/10-distributed-systems.md) | List every place the system assumes a remote call succeeds | Inject a fault into one dependency and observe what actually happens |
| [CS11 Databases](../computer-science-in-ai-curriculum/talks/11-database-systems.md) (deeper pass) | Read the plans for the ten most expensive queries | Complete a full [database playbook](../scientific-method/database.md) investigation |
| [CS16 Cryptography](../computer-science-in-ai-curriculum/talks/16-cryptography.md) | Audit how the codebase hashes passwords, signs tokens, and stores secrets | |
| [SDA14](../computer-science-software-design-and-architecture/curriculum/part-4/part-4-14--core-system-design-concepts/README.md)–[SDA20](../computer-science-software-design-and-architecture/curriculum/part-5/part-5-20--caching-strategies/README.md) System design and data at scale | Write a CAP trade-off memo for one real feature | |

**Milestone:** the Mid-level leads a design review for a real feature, with an ADR (see [SDA29](../computer-science-software-design-and-architecture/curriculum/part-8/part-8-29--understanding-software-architecture/README.md) homework).

### Q4: How AI works

| Material | Codebase connection | Scientific Method practice |
|---|---|---|
| [CS17 Artificial Intelligence](../computer-science-in-ai-curriculum/talks/17-artificial-intelligence.md), [CS18 Machine Learning](../computer-science-in-ai-curriculum/talks/18-machine-learning.md), [CS20 NLP](../computer-science-in-ai-curriculum/talks/20-natural-language-processing.md) | Collect ten examples of the team's AI tools being confidently wrong, and classify *why* | Run an experiment on the team's own AI workflow. For example, does adding a project instructions file reduce review rework? Measure it. |
| [CS27 Ethics, Policy & Society](../computer-science-in-ai-curriculum/talks/27-computing-ethics-policy-and-society.md) | Review how the team's AI usage handles customer data and licensing | |
| [SDA21](../computer-science-software-design-and-architecture/curriculum/part-6/part-6-21--asynchronism/README.md)–[SDA24](../computer-science-software-design-and-architecture/curriculum/part-6/part-6-24--monitoring-observability/README.md) Async, protocols, antipatterns, observability | Find one performance antipattern in production and fix it | Instrument before and after, and report the measured change |

**Milestone:** the Mid-level gives a talk to the wider engineering group on where AI fails in *this* codebase and how the team catches it.

## Session formats

| Format | How it works | Good for |
|---|---|---|
| **Talk and discuss** (60 min) | Mid-level reads the talk outline beforehand. Senior gives or co-gives a condensed version (25 min), then 35 min on "where does this show up in our systems?" | Introducing a new subject |
| **Explain it back** (30 min) | Mid-level explains a concept to the Senior at a whiteboard, with no notes or AI. Senior asks "why" until the explanation runs out. | Finding the real edges of understanding |
| **Experiment log review** (30 min) | Senior reviews a Mid-level's recent investigation using the [review checklist](../scientific-method/experiment-log-template.md#review-checklist) | Building diagnostic judgment |
| **Design review** (60 min) | Mid-level presents a design. Senior and peers challenge trade-offs. | Architecture and communication skills |
| **Incident retrospective reading** (45 min) | Read a public postmortem or an internal one together, and name the CS concept at its root | Connecting theory to real failures |
| **AI output teardown** (30 min) | Take a piece of AI-generated code or advice, and find everything wrong with it using the quarter's subject | Making the "dangerous weakness" concrete |

## Cadence

- **Weekly:** one 60-minute session per pod (1 Senior with 2–4 Mid-levels), rotating through the formats above
- **Every two weeks:** a 30-minute 1:1 between Senior and each Mid-level for experiment log review and career conversation
- **Monthly:** one design review open to the wider team
- **Quarterly:** milestone check and plan for the next quarter. See [program operations](program-operations.md).

Mid-levels should expect to spend roughly 2–3 hours a week on this track: the session, reading, and the codebase exercise.

## Using AI in this track

AI is a good study partner here if it's used the right way.

- **Do** use AI as a Socratic tutor: "Quiz me on B-tree index behavior. Don't give answers until I've tried."
- **Do** ask AI to generate practice problems, counter-examples, and edge cases.
- **Do** have AI critique your design docs and argue the other side.
- **Don't** let AI summaries replace reading the talk outline or doing the homework.
- **Check** understanding without AI. The "explain it back" sessions exist for this reason.

## What Seniors are responsible for

- Showing up consistently. Cancelled sessions are the fastest way to kill the program.
- Connecting every subject to real systems and real incidents, not just the outline
- Reviewing experiment logs for reasoning quality, not only whether the bug got fixed
- Saying "I don't know, let's find out" when it's true, and then running the experiment together
- Watching for Mid-levels overloaded by their own Junior mentoring, and raising it with the manager

## Anti-patterns

| Anti-pattern | Why it hurts | Instead |
|---|---|---|
| Turning sessions into ticket help | The CS content never gets covered | Keep ticket help in ad-hoc time. Protect the session for the track. |
| Lecturing for the full hour | Passive listening doesn't build judgment | Cap talking at 25 minutes, and spend the rest on application |
| Skipping the codebase connection | Theory stays abstract and fades | Every subject ends with "where is this in our code?" |
| Senior as oracle | Mid-levels learn to defer instead of reason | Answer questions with experiments when you can |
| Measuring by talks completed | Rewards box-ticking | Measure with the milestones: talks given, designs reviewed, investigations written up |

## Readiness for Senior

This track is also a path to promotion. By the end of the year, the evidence for Senior readiness is on record: talks delivered, design reviews led, ADRs written, experiment logs reviewed, and Juniors successfully mentored in the [Mid → Junior track](mid-to-junior.md). Managers should use that record in promotion discussions.
