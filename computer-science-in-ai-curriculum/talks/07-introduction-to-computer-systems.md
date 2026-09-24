# Introduction to Computer Systems

**Backbone Course #7** · **Duration:** 50 minutes

## The One-Sentence Pitch
Between "source code" and "running program" sits a whole hidden system — memory laid out in specific regions, a translation pipeline, and an operating system standing guard — and understanding it explains bugs that otherwise look like magic.

## Audience & Prerequisites
This talk is for learners comfortable with basic programming (functions, variables) and ideally some exposure to how a CPU executes instructions (the Computer Organization talk is helpful but not required).

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: "segfault" and "stack overflow" as clues |
| 0:05–0:16 | The memory model: stack vs. heap |
| 0:16–0:27 | From source code to executable |
| 0:27–0:38 | Processes and the OS as mediator |
| 0:38–0:47 | Why concurrency is dangerous |
| 0:47–0:50 | Close |

### Hook: "segfault" and "stack overflow" as clues
Point out that two of the most infamous error messages in programming — "segmentation fault" and "stack overflow" — are literally named after the system-level concepts this talk covers, yet most self-taught programmers treat them as unexplained crashes rather than precise, meaningful diagnoses. A segfault means a program tried to access memory it doesn't own; a stack overflow means a program's call stack (often via unbounded recursion) grew past its allotted space — both are direct, readable consequences of how a program's memory is organized. The promise of this talk: by the end, these error names will describe exactly what went wrong, not just that something did.

### The memory model: stack vs. heap
Every running program's memory is divided into regions, and two matter most day-to-day: the stack, which holds function call frames (local variables, return addresses) and grows/shrinks automatically as functions are called and return; and the heap, which holds dynamically allocated data that outlives a single function call and must be explicitly managed (allocated, and in some languages, freed). Walk through a small example: calling `foo()` which calls `bar()` pushes a new frame onto the stack for each; when `bar()` returns, its frame is popped — automatic, fast, and strictly nested (which is also exactly why deep/unbounded recursion causes a stack overflow, each call adds a frame with nowhere to shrink). Contrast with heap allocation: a heap-allocated object survives after the function that created it returns, which is powerful (you can return a large structure without copying it) but also the direct cause of memory leaks (forgetting to free heap memory that's no longer needed) and dangling pointers (using heap memory after it's been freed). This single distinction — automatic, short-lived, nested (stack) versus manual or garbage-collected, flexible-lifetime (heap) — is the root cause of a huge fraction of the memory bugs programmers encounter, regardless of language.

### From source code to executable
Turning human-written source code into something the CPU can run happens in stages: compiling translates source code into assembly/object code (the CPU-specific low-level instructions from the architecture talk), assembling turns that into raw machine code, and linking combines multiple compiled files plus any needed libraries into a single runnable executable, resolving references between them (e.g., "this file calls a function defined in that other file"). Use a concrete mental model: writing a book chapter (source), translating it (compiling), and then binding several chapters plus a shared appendix into one final book (linking) — the linker's job is specifically to stitch separately-compiled pieces together and resolve "who defines what." Mention that interpreted languages (Python, JavaScript) skip straight ahead of this pipeline at execution time via an interpreter, or blend it with just-in-time compilation, but the underlying concepts — translate human-readable code into something directly executable, then resolve cross-file references — still apply, just at a different point in time. The practical payoff: linker errors ("undefined reference") and "why is my program a huge file" (statically linked libraries) both trace directly back to this pipeline.

### Processes and the OS as mediator
A process is a running instance of a program, and the operating system gives every process the illusion that it has the entire machine to itself — its own private memory space, its own view of the CPU — even though many processes are actually sharing the same physical hardware simultaneously. This illusion is what prevents one buggy or malicious program from directly reading or corrupting another program's memory; the OS enforces isolation, and any attempt by a process to touch memory outside its own allotted space is exactly what triggers a segmentation fault (the answer to the segfault half of the opening hook). Frame the OS's role plainly: it's the mediator between every running program and the actual, shared, finite hardware — deciding which process runs when (a preview of scheduling in the Operating Systems talk), which memory belongs to whom, and enforcing the boundaries so multiple programs can safely coexist.

### Why concurrency is dangerous
Concurrency means multiple threads (or processes) making progress on overlapping timeframes, and it becomes dangerous the moment they share mutable state without coordination — because a thread's operations can be interrupted and interleaved with another thread's at almost any point. Walk through the classic shared-counter example live: two threads both run `count = count + 1`, but "read count, add one, write count back" is actually three separate steps — if both threads read the same starting value before either writes back, one increment is silently lost, and the final count is wrong despite each line of code looking perfectly correct in isolation. This is a race condition — the outcome depends on unpredictable timing rather than the logic of the code — and it's genuinely dangerous because it often doesn't show up in testing (timing has to line up just wrong) yet causes real, hard-to-reproduce bugs in production. Note this is deliberately left as "here's the danger, be aware" rather than "here's the fix" — locks, semaphores, and other synchronization tools that solve this problem are covered in depth in the Operating Systems talk.

### Close
Trace the full path covered: memory is organized into stack and heap regions with very different lifetime rules, source code travels through a compile/assemble/link pipeline before it can run, the OS wraps every process in an isolated illusion of owning the whole machine, and sharing that illusion's underlying state across threads without care produces race conditions. Every one of these ideas turns a previously mysterious crash or bug into something diagnosable — that diagnostic instinct is the real payoff of learning systems-level thinking.

## Key Takeaway
Bugs that look like unexplainable magic — segfaults, stack overflows, mysteriously wrong counters under load — are usually precise, nameable consequences of how memory, compilation, and concurrency actually work under the hood.

## Go Deeper
- [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.033 — Computer System Engineering](https://ocw.mit.edu/courses/6-033-computer-system-engineering-spring-2018/pages/syllabus/)
