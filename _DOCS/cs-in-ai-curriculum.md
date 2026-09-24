# Computer Science in AI: Curriculum Guide

[Back to docs](README.md) · [Open the curriculum](../computer-science-in-ai-curriculum/README.md)

## What it is

Seven university computer science programs were turned into checklists of standalone 50-minute talks. Every talk title traces back to something the source actually states. Across the seven schools that comes to ~791 courses and ~3,307 talks.

On top of that raw layer sits the **backbone**: the 27 subjects that show up at most schools under different course numbers. Each one has a full talk outline. Five algorithm topics were too big to fold into a broader talk, so they got their own deep-dives (28–32). That makes 32 fully outlined talks.

## The priority test

Each talk is ranked against one question:

> If a developer is weak on this subject, does leaning on AI make that weakness *dangerous* (confident, wrong, hard-to-catch output), or does it just make them *less deep*, with AI covering the gap?

| Priority | Count | Talks |
|---|---|---|
| **Critical** | 13 | 1 Intro to Programming · 2 Data Structures · 3 Algorithms · 4 Discrete Math · 7 Computer Systems · 11 Databases · 14 Software Engineering · 15 Security · 17 AI · 18 Machine Learning · 20 NLP · 23 HCI · 27 Ethics |
| **Important** | 12 | 5 Theory of Computation · 6 Computer Architecture · 8 Operating Systems · 9 Networks · 10 Distributed Systems · 12 Programming Languages · 16 Cryptography · 19 Deep Learning · 25 Parallel/HPC · 28 Dijkstra · 29 BFS/DFS · 30 Sorting & Searching |
| **Optional** | 7 | 13 Compilers · 21 Computer Vision · 22 Graphics · 24 Robotics · 26 Information Theory · 31 FFT · 32 PageRank |

The curriculum README gives the one-line reason behind each ranking. Treat the list as a guide for spending limited study time. "Optional" does not mean unimportant to the field.

## Algorithm coverage

Thirteen commonly cited "top algorithms" were checked against every school file. Some earned a dedicated talk. Others were taught in depth inside an existing talk:

| Algorithm | Where it lives |
|---|---|
| Dijkstra's shortest path | Talk 28 (dedicated) |
| BFS and DFS | Talk 29 (dedicated) |
| Binary search, quicksort, mergesort | Talk 30 (dedicated) |
| Fast Fourier Transform | Talk 31 (dedicated) |
| PageRank | Talk 32 (dedicated) |
| RSA, SHA-256 | Inside Talk 16, Cryptography |
| A\* search | Inside Talk 17, Artificial Intelligence |
| K-Means clustering | Inside Talk 18, Machine Learning |
| Huffman coding | Inside Talk 26, Information Theory |

## The school checklists

| School | Courses | Talks | Source |
|---|---|---|---|
| Amherst College | 34 | ~85 | Department curriculum page |
| Bowling Green State University | 72 | ~472 | Model course syllabi, fetched per course |
| Purdue University | 39 | ~340 | Canonical syllabi, fetched per course |
| MIT OpenCourseWare | 69 (from 154 raw hits) | ~330 | Curated to drop duplicate years and pure-EE courses |
| Harvard SEAS | 53 | ~155 | SEAS course listing |
| Stanford | 50 (of ~150 listed) | ~157 | Matched against the 2002–03 Registrar bulletin |
| Carnegie Mellon SCS | 574 | ~1,768 | Full catalog across 9 departments |

Use the school files when you want more depth on a backbone subject than one 50-minute talk allows. Open the file and search for the subject name.

## Using it

- **Giving a talk:** open any file in `talks/`. The segment table is your run-of-show, and the prose under each segment is your speaker notes.
- **Building a study plan:** start with the 13 Critical talks. Add Important talks that match your role. Pick up Optional talks when a project needs them.
- **In mentoring:** the Senior → Mid-level track draws on this curriculum directly. See [senior-to-mid.md](../mentoring/senior-to-mid.md).
- **In diagnostics:** many Scientific Method playbooks point back to a talk for the theory behind a failure mode. Talk 07 explains memory and concurrency bugs, and Talk 11 explains query plans.
