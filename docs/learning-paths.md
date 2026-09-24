# Learning Paths

[Back to docs](README.md)

These are suggested orders through both curricula. They line up with the mentoring tiers in [mentoring/](../mentoring/README.md), so a Mid-level developer studying the Mid path is also teaching from the Junior path.

Abbreviations: **CS##** is a talk in the [CS in AI curriculum](../computer-science-in-ai-curriculum/README.md). **SDA##** is a topic in the [Software Design & Architecture curriculum](../computer-science-software-design-and-architecture/README.md).

## Junior path: literacy, code quality, and working with AI

Goal: read, direct, and verify code in the team's codebases, and use AI without being fooled by it. The mentor is a Mid-level developer. See [mid-to-junior.md](../mentoring/mid-to-junior.md).

| Order | Item | Why now |
|---|---|---|
| 1 | [CS01 Introduction to Programming](../computer-science-in-ai-curriculum/talks/01-introduction-to-programming.md) | Decomposition is the skill behind every good prompt |
| 2 | [SDA1 Clean Code Principles](../computer-science-software-design-and-architecture/curriculum/part-1/part-1-1--clean-code-principles/README.md) | The standard you hold AI-generated code to |
| 3 | [CS02 Data Structures](../computer-science-in-ai-curriculum/talks/02-data-structures.md) | So you can spot the linear scan where a hash lookup belongs |
| 4 | [CS14 Software Engineering](../computer-science-in-ai-curriculum/talks/14-software-engineering.md) | Testing and review become the human's main job |
| 5 | [SDA2](../computer-science-software-design-and-architecture/curriculum/part-1/part-1-2--structured-programming/README.md)–[SDA4](../computer-science-software-design-and-architecture/curriculum/part-1/part-1-4--object-oriented-programming/README.md) Structured, Functional, OOP | Recognizing the paradigm the codebase (and the AI) is using |
| 6 | [CS15 Computer & Network Security](../computer-science-in-ai-curriculum/talks/15-computer-and-network-security.md) | AI-generated code has repeatable security blind spots |
| 7 | [Scientific Method overview](../scientific-method/README.md) | Debugging by experiment instead of by guessing, or by asking the AI to guess |
| 8 | [CS11 Database Systems](../computer-science-in-ai-curriculum/talks/11-database-systems.md) | Most application bugs eventually touch the data layer |

## Mid-level path: computer science depth and design judgment

Goal: understand *why* systems behave the way they do, design with that knowledge, and be able to teach Juniors. The mentor is a Senior developer. See [senior-to-mid.md](../mentoring/senior-to-mid.md).

| Quarter | CS in AI | Software Design & Architecture |
|---|---|---|
| Q1: Foundations of reasoning | [CS03 Algorithms](../computer-science-in-ai-curriculum/talks/03-algorithms.md), [CS04 Discrete Math](../computer-science-in-ai-curriculum/talks/04-discrete-mathematics-for-cs.md), [CS29 BFS/DFS](../computer-science-in-ai-curriculum/talks/29-graph-traversal-bfs-dfs.md), [CS30 Sorting & Searching](../computer-science-in-ai-curriculum/talks/30-sorting-and-searching-fundamentals.md) | SDA5 Paradigms, SDA6–8 Principles, SOLID, DRY/YAGNI |
| Q2: How machines run code | [CS07 Computer Systems](../computer-science-in-ai-curriculum/talks/07-introduction-to-computer-systems.md), [CS08 Operating Systems](../computer-science-in-ai-curriculum/talks/08-operating-systems.md), [CS09 Networks](../computer-science-in-ai-curriculum/talks/09-computer-networks.md) | SDA9 Design Patterns, SDA10–13 Architecture |
| Q3: Systems at scale | [CS10 Distributed Systems](../computer-science-in-ai-curriculum/talks/10-distributed-systems.md), [CS11 Databases](../computer-science-in-ai-curriculum/talks/11-database-systems.md) (deeper pass), [CS16 Cryptography](../computer-science-in-ai-curriculum/talks/16-cryptography.md) | SDA14–20 System Design and Data at Scale |
| Q4: How AI works | [CS17 AI](../computer-science-in-ai-curriculum/talks/17-artificial-intelligence.md), [CS18 Machine Learning](../computer-science-in-ai-curriculum/talks/18-machine-learning.md), [CS20 NLP](../computer-science-in-ai-curriculum/talks/20-natural-language-processing.md), [CS27 Ethics](../computer-science-in-ai-curriculum/talks/27-computing-ethics-policy-and-society.md) | SDA21–24 Async, Protocols, Antipatterns, Observability |

Across all four quarters, the Mid-level developer runs at least one full [Scientific Method](../scientific-method/README.md) investigation per quarter and writes it up with the [experiment log](../scientific-method/experiment-log-template.md).

## Senior path: architecture and multiplying others

Goal: own cross-cutting technical decisions and grow Mid-levels into future Seniors.

| Focus | Items |
|---|---|
| Cloud and resilience | SDA25–28 Cloud Design Patterns |
| The architect role | SDA29–33: architecture levels, soft skills, frameworks, security, DevOps |
| Specialist depth, as the role needs | CS05 Theory of Computation, CS06 Architecture, CS12 Programming Languages, CS19 Deep Learning, CS25 Parallel/HPC |
| Capstone | The [Software Design & Architecture capstone](../computer-science-software-design-and-architecture/README.md#suggested-capstone), done as a reference build that Mid-levels can study |
| Teaching | Deliver the talks and topics above to Mid-levels. See [senior-to-mid.md](../mentoring/senior-to-mid.md) |

## Specialist electives

Pick these up when a project calls for them: CS13 Compilers, CS21 Computer Vision, CS22 Graphics, CS24 Robotics, CS26 Information Theory, CS28 Dijkstra, CS31 FFT, CS32 PageRank.
