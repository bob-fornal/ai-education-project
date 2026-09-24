# Parallel & High-Performance Computing

**Backbone Course #25** · **Duration:** 50 minutes

## The One-Sentence Pitch
Since single-core speed stopped improving around 2005, performance gains mostly come from doing many things at once — and parallelism has hard mathematical limits and its own unique bugs.

## Audience & Prerequisites
For learners with basic programming experience who understand what a thread is at a high level; no prior systems or concurrency coursework required.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: why parallelism became necessary |
| 0:05–0:17 | Shared-memory vs. distributed-memory models |
| 0:17–0:30 | Amdahl's Law: the math of speedup |
| 0:30–0:42 | Concrete pitfalls: race conditions and false sharing |
| 0:42–0:47 | Worked example: parallelizing a simple task |
| 0:47–0:50 | Key takeaway and close |

### Hook: why parallelism became necessary
Open with a short history: for decades, "make software faster" mostly meant "wait for next year's faster chip," because clock speeds climbed steadily (roughly following Moore's Law expectations). Around 2005, that stopped — clock speeds plateaued around 3-4 GHz because pushing them higher generates unmanageable heat (the so-called "power wall"). Chip makers pivoted to putting more cores on a chip instead of making one core faster. The consequence for every programmer: free performance from hardware alone stopped happening, and taking advantage of multiple cores now requires software explicitly written to do things in parallel. Frame the rest of the talk as answering "how do we actually do that, and what goes wrong?"

### Shared-memory vs. distributed-memory models
Introduce the two dominant models for organizing parallel work. Shared-memory: multiple threads run on one machine and all read/write the same RAM, so communication is just reading and writing shared variables — fast and simple to reason about in principle, but this is exactly what opens the door to race conditions when two threads touch the same memory unsynchronized. Distributed-memory: separate machines (or processes) each have their own private memory and must explicitly send messages to share data — nothing is implicitly shared, which avoids shared-memory bugs but adds real communication cost and latency. Give a concrete pairing: a multi-threaded program on your laptop doing image processing across its 8 cores is shared-memory; a cluster of 1000 machines training a large model together, passing gradients over a network, is distributed-memory. Note that many real systems are hybrids — clusters of multi-core machines, parallel within a machine and distributed across machines.

### Amdahl's Law: the math of speedup
Present Amdahl's Law as the sobering reality check on how much parallelism can actually buy you. State it conceptually: if a fraction of your program must run sequentially (call it the serial portion), that portion puts a hard ceiling on your maximum possible speedup, no matter how many cores you throw at the parallelizable rest. Walk through a concrete case: if 10% of a program's runtime is inherently sequential, the theoretical maximum speedup is 10x — even with infinite cores, you can never beat that, because the 10% never shrinks no matter how much you parallelize the other 90%. Make it visceral with numbers: going from 1 to 10 cores on that program gets you a meaningful boost, but going from 100 to 1000 cores barely moves the needle, because you're already close to the 10x ceiling. The takeaway to land: reducing the serial fraction of your program is often more valuable than adding more cores, and "just add more cores" has diminishing, then vanishing, returns.

### Concrete pitfalls: race conditions and false sharing
Revisit the classic shared-counter race condition from a systems angle: two threads both read a counter's current value, both increment their local copy, and both write back — one increment gets silently lost because the operations interleaved instead of happening atomically. Reinforce why this matters more in HPC: at scale, with many threads hammering shared state, these lost updates become frequent and nondeterministic, making bugs that are brutally hard to reproduce. Then introduce false sharing as a subtler, purely performance-level trap: CPU caches move data in fixed-size chunks called cache lines (commonly 64 bytes); if two threads modify separate variables that happen to sit on the same cache line, the cache-coherency hardware treats it as if they're fighting over the same data, constantly invalidating and reloading the line even though the threads never touch each other's actual variable. The fix — padding data structures so hot variables land on separate cache lines — illustrates that parallel performance bugs can exist purely at the hardware cache level, invisible in the source code's logic.

### Worked example: parallelizing a simple task
Walk through parallelizing a simple sum-of-an-array task end to end: split the array into chunks, one per thread, each thread sums its chunk locally (no shared-memory contention during this phase, so no race condition risk), then combine the partial sums at the end (a small sequential step). Point out this design deliberately minimizes shared-memory touches to avoid both race conditions and false sharing, and note that the "combine" step is exactly the serial fraction Amdahl's Law warns about — small here, but not zero, and it caps the achievable speedup.

### Key takeaway and close
Bring it together: parallel programming is now a required skill because clock speeds plateaued, but it comes with a hard mathematical ceiling (Amdahl's Law) and its own class of bugs (races) and performance traps (false sharing) that don't exist in single-threaded code.

## Key Takeaway
More cores only help up to the limit set by your program's unavoidable sequential portion — and parallel code brings entirely new failure modes, from race conditions to cache-level false sharing, that single-threaded programmers never have to think about.

## Go Deeper
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.338J — Parallel Computing](https://ocw.mit.edu/courses/18-337j-parallel-computing-fall-2011/pages/syllabus/)
