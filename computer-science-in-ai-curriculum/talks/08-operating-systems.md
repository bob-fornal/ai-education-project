# Operating Systems

**Backbone Course #8** · **Duration:** 50 minutes

## The One-Sentence Pitch
The operating system is the layer of software whose entire job is arbitrating access to shared, limited hardware — CPU time, memory, disk — fairly, safely, and (mostly) invisibly to every program running on top of it.

## Audience & Prerequisites
This talk is for learners who've seen the Introduction to Computer Systems talk (processes, the stack/heap, and race conditions) or have equivalent background; no prior OS coursework is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: the OS as an invisible referee |
| 0:05–0:14 | Processes vs. threads |
| 0:14–0:23 | CPU scheduling |
| 0:23–0:33 | Virtual memory and paging |
| 0:33–0:44 | Synchronization and deadlock |
| 0:44–0:50 | File systems and close |

### Hook: the OS as an invisible referee
Ask the audience to count how many programs are running on a typical laptop right now — browser tabs, background updates, music, chat apps — and note that a machine likely has far more processes wanting attention than it has CPU cores to give them. The operating system is the referee that makes this feel seamless: deciding whose turn it is to run, giving each program the illusion of private memory, and keeping one program's mistakes from crashing every other program on the machine. This talk walks through the OS's core responsibilities in turn — who runs, where things live in memory, how shared resources are coordinated safely, and how a disk becomes organized "files" — using the process/segfault concepts from the systems talk as the running foundation.

### Processes vs. threads
A process is an isolated running program with its own private memory space, enforced by the OS, while a thread is a unit of execution *within* a process that shares that process's memory with any other threads in the same process — meaning multiple threads can directly see and modify the same variables, for better and worse. This is a direct tradeoff: processes are safer (a crash or memory corruption in one process cannot directly touch another) but more expensive to create and to communicate between (they need explicit inter-process communication); threads are cheaper to create and communicate through shared memory instantly, but that same shared memory is exactly what creates race conditions like the shared-counter example from the systems talk. Use a workplace analogy: processes are like separate companies (which need formal contracts/channels to exchange information) while threads are like coworkers sharing one open-plan office (fast, informal communication, but they can bump into and corrupt each other's stuff without careful etiquette). The practical rule of thumb this sets up: reach for threads when you need speed and are willing to carefully synchronize shared state; reach for separate processes when isolation and fault-tolerance matter more than raw efficiency.

### CPU scheduling
With more runnable processes/threads than CPU cores, the OS's scheduler decides who gets the CPU and for how long, balancing competing goals: throughput (total work done per unit time), fairness (no process starves indefinitely), and responsiveness (interactive programs like a UI shouldn't feel laggy just because something else is doing heavy background work). Contrast two concrete policies: round-robin gives each ready process a fixed small time slice in turn, cycling through everyone — simple and fair, good for responsiveness, but not necessarily optimal for total throughput; priority scheduling instead always runs the highest-priority ready process first, which lets urgent work (a video call) preempt unimportant work (a background download), at the risk of starving low-priority work if not managed carefully. Note that real schedulers (in Linux, Windows, macOS) are considerably more sophisticated hybrids of these ideas, but the underlying tension — throughput vs. fairness vs. responsiveness, no policy maximizes all three — is the durable lesson, and it's a tension that reappears anywhere resources are shared (traffic lights, customer service lines, network bandwidth).

### Virtual memory and paging
Virtual memory is the mechanism that gives every process the illusion of having its own large, private, contiguous address space, even though physical RAM is shared among all running processes and is usually smaller than the sum of what every process "thinks" it has. Paging implements this by dividing memory into fixed-size chunks called pages; each process's virtual pages are mapped to physical memory locations by the OS (via a page table), and pages not currently needed can be swapped out to disk, freeing physical RAM for pages that are actively in use. This is precisely how a process's memory can be isolated from every other process's (mapped to entirely different physical locations, so one process literally cannot address another's memory) while still letting the machine run more total programs than would fit in RAM if each needed a dedicated, exclusive chunk. Connect back to the previous systems talk: a segmentation fault is what happens when a process's virtual address doesn't map to any valid page it's allowed to access — the OS catches the invalid mapping and kills the offending access rather than letting it corrupt other memory.

### Synchronization and deadlock
When threads share memory, the OS provides synchronization primitives to coordinate safe access: a lock (mutex) allows only one thread at a time into a critical section of code, directly preventing the interleaved read-modify-write race condition seen with the shared counter; a semaphore generalizes this to allow up to N threads at once, useful for managing a limited pool of resources (like N available database connections). Introduce deadlock as the classic failure mode this coordination can itself cause: if thread A holds lock 1 and waits for lock 2, while thread B holds lock 2 and waits for lock 1, neither can ever proceed — a frozen standoff with no timeout by default. Make deadlock's four classic conditions memorable via the standoff itself: mutual exclusion (only one holder per lock), hold-and-wait (holding one while waiting for another), no preemption (can't forcibly take a lock away), and circular wait (A waits on B who waits on A) — and note that breaking any single one of these conditions (e.g., always acquiring locks in the same global order, eliminating circular wait) is enough to prevent deadlock entirely. The larger point: synchronization tools solve one class of dangerous bug (races) but introduce a new one (deadlock) if used carelessly — safety in concurrent systems requires deliberate discipline, not just "add a lock and move on."

### File systems and close
Briefly land on file systems as the OS's answer to "how do I make a spinning disk or flash chip, which is really just a giant array of raw bytes/blocks, look like organized, named files and folders?" The file system layer tracks which physical blocks belong to which named file, exposes a hierarchical directory structure, and handles concerns like what happens if the power cuts out mid-write — all so that "open myFile.txt" can be a simple, reliable operation rather than a program having to track raw disk addresses itself. Close by tying the whole talk together: every topic covered — processes/threads, scheduling, virtual memory, synchronization, file systems — is really the same referee mediating a different shared, limited resource (CPU, memory, safe access to shared data, disk), which is the unifying job description of an operating system.

## Key Takeaway
An operating system's job, underneath every specific mechanism, is always the same: safely and fairly mediate access to a shared, limited resource (CPU time, memory, locks, disk blocks) — and most OS bugs and design tradeoffs make more sense once you ask "which resource is being shared here, and how is contention being managed?"

## Go Deeper
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.828 — Operating System Engineering](https://ocw.mit.edu/courses/6-828-operating-system-engineering-fall-2012/pages/syllabus/)
