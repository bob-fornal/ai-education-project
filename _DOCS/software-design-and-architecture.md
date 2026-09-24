# Software Design & Architecture: Curriculum Guide

[Back to docs](README.md) · [Open the curriculum](../computer-science-software-design-and-architecture/README.md)

## What it is

A 33-topic course built from three roadmap.sh guides: [Software Design & Architecture](https://roadmap.sh/software-design-architecture), [System Design](https://roadmap.sh/system-design), and [Software Architect](https://roadmap.sh/software-architect). Topics are ordered to be taught in sequence. Each has a ~55-minute session plan, homework, and a free companion resource that was checked for paywalls.

Session times are for *teaching* a topic. Actually learning it takes a lot longer.

## The eight parts

| Part | Topics | Theme | What a learner should come away with |
|---|---|---|---|
| 1. Foundations of Code Quality | 1–5 | Clean code, structured, functional, OOP, paradigms | Code that's cheap to change, and the ability to pick a paradigm on purpose |
| 2. Design Principles & Patterns | 6–9 | Core principles, SOLID, DRY/YAGNI, GoF & PoSA | A shared vocabulary for design, and knowing when a pattern is overkill |
| 3. Architectural Foundations | 10–13 | Principles, styles, patterns, enterprise patterns | Drawing boundaries between policy and detail; MVC, DDD, CQRS, microservices |
| 4. System Design Fundamentals | 14–17 | CAP, consistency, DNS/CDN/LB, scaling | Reasoning about trade-offs under partition and load |
| 5. Data at Scale | 18–20 | Databases at scale, NoSQL, caching | Picking and shaping data stores for access patterns |
| 6. Async & Distributed Communication | 21–24 | Queues, protocols, antipatterns, observability | Building systems that degrade gracefully and can be seen into |
| 7. Cloud Design Patterns | 25–28 | Messaging, data, resiliency, security patterns | Circuit breakers, bulkheads, event sourcing, valet key |
| 8. The Software Architect Role | 29–33 | Architecture, soft skills, frameworks, security, DevOps | ADRs, requirements, decision memos, and the ops knowledge an architect needs |

## Per-topic material

Each topic's `README.md` under `curriculum/part-N/` has the pitch, learning objectives, session outline, and segment notes. The root README of the sub-project adds a one-paragraph summary and 2–3 homework assignments per topic.

Worked homework material and starter code exist for:

- **Topic 9, Design Patterns:** a refactor-me file (`homework2_before.py`) and a PoSA concurrency starter
- **Topic 12, Architectural Patterns:** the same order feature built as MVC and as CQRS, for comparison
- **Topic 17, Scaling Applications:** externalizing session state to Redis, and a minimal service registry

## Capstone

The sub-project README closes with a multi-week capstone. Learners build a small real service (a URL shortener, ticketing system, or job board) that uses:

- An explicit architectural style and patterns (Part 3)
- Clean code and SOLID (Parts 1–2)
- Horizontal scaling with caching and a message queue (Parts 4–6)
- At least two reliability or cloud patterns (Part 7)
- An ADR and UML diagrams (Part 8)

## How it connects to the rest of the repo

- **CS in AI curriculum:** that curriculum explains *why* systems behave the way they do (data structures, OS, networks, distributed systems). This one explains *how to design with that knowledge*. The [learning paths](learning-paths.md) interleave them.
- **Scientific Method:** Topics 23 (Performance Antipatterns), 24 (Monitoring & Observability), and 27 (Reliability & Resiliency) supply the vocabulary the diagnostic playbooks lean on. You can't form a good hypothesis about a retry storm if you've never heard of one.
- **Mentoring:** Parts 3–8 are the core of what Seniors teach Mid-levels in the [Senior → Mid-level track](../mentoring/senior-to-mid.md).
