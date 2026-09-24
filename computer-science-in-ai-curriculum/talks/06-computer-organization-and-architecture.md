# Computer Organization & Architecture

**Backbone Course #6** · **Duration:** 50 minutes

## The One-Sentence Pitch
Underneath every program, from a "hello world" script to a neural network, is a machine built from millions of simple on/off switches, executing one small instruction at a time, faster than intuition can grasp.

## Audience & Prerequisites
This talk is for learners with basic programming experience and no assumed background in electronics or hardware; comfort with binary numbers is helpful but will be introduced from scratch.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: what is actually happening when code "runs"? |
| 0:05–0:14 | Binary representation of data |
| 0:14–0:23 | From logic gates to the ALU |
| 0:23–0:33 | The fetch-decode-execute cycle |
| 0:33–0:44 | The memory hierarchy |
| 0:44–0:50 | Pipelining and close |

### Hook: what is actually happening when code "runs"?
Ask the audience to imagine peeling back every layer of abstraction from a line of Python down to the physical machine — the language, the interpreter, the operating system, and finally, actual electricity moving through silicon. Most programmers spend an entire career comfortably above this layer, and that's fine day-to-day, but understanding it once builds real intuition for *why* certain things are slow, why certain bugs happen (overflow, alignment), and why hardware choices (cache size, core count) matter for performance. This talk goes from the smallest possible unit — a single bit — up to a running program, in five short hops.

### Binary representation of data
Computers represent everything — numbers, text, images, instructions themselves — as sequences of bits (0s and 1s), because the underlying hardware is built from switches that are most reliably built as two-state (on/off) devices rather than many-state ones. Show basic binary-to-decimal counting (0000 to 1111 as 0 to 15) and point out that an 8-bit byte therefore has exactly 256 possible values, which is why a standard unsigned byte maxes out at 255 — a very concrete, countable fact that explains real bugs like integer overflow. Briefly mention that negative numbers, floating-point numbers, and characters (via encodings like ASCII/Unicode) are all just different agreed-upon *interpretations* of the same underlying bit patterns — the hardware doesn't inherently know if a given byte is "the number 65" or "the letter A," software convention decides. This sets up the entire rest of the talk: everything from here on is bits, interpreted consistently by agreement between hardware and software.

### From logic gates to the ALU
A logic gate is a tiny physical circuit that computes a basic boolean operation — AND, OR, NOT — on its inputs, and remarkably, every arithmetic and logical operation a computer performs is built by wiring together enormous numbers of these simple gates. Show, at a conceptual level, how an AND gate plus an XOR gate can be combined into a single-bit adder, and how chaining several of these together (with carry bits flowing between them) builds a multi-bit adder capable of adding two full numbers — the same "compose small pieces into something bigger" idea seen with functions in the programming talk. The Arithmetic Logic Unit (ALU) is the component that bundles many of these gate circuits together to perform all the basic operations a CPU needs (add, subtract, compare, bitwise operations), and it's worth stating plainly: there is no "magic" inside a computer's math — it's boolean logic, physically wired, all the way down.

### The fetch-decode-execute cycle
A CPU runs a program by repeating a simple three-step cycle, over and over, billions of times per second: fetch the next instruction from memory, decode it to figure out what operation and operands it specifies, and execute it (often using the ALU), then move to the next instruction. Walk through one tiny concrete instruction — something like "add the values in two registers and store the result" — and trace it through all three steps explicitly, making clear that even a single line of high-level code usually compiles down to several of these low-level instructions. Emphasize the rhythm and simplicity of the cycle: a modern CPU isn't doing anything conceptually different from this simple loop, it is just doing it at a staggering scale of speed (billions of cycles per second) and increasingly, doing several cycles in parallel — a preview of pipelining.

### The memory hierarchy
Not all memory is equal: registers (inside the CPU) are the fastest and smallest, then cache (small, very fast, sits close to the CPU), then RAM (much larger, noticeably slower), then disk/SSD (huge, but far slower still) — and the whole system is designed around one empirical fact: programs exhibit locality of reference, meaning they tend to reuse the same data and access nearby data repeatedly in short windows of time. Because of locality, keeping recently- and nearby-used data in fast cache (rather than fetching everything from slow RAM every time) gives most of the speed benefit of "all memory is fast" without the impossible cost of actually building all memory that fast. Use a library analogy: the few books on your desk (registers/cache) are instant to grab, the shelf across the room (RAM) takes a short walk, and interlibrary loan (disk) takes days — and any well-organized workflow keeps the frequently-needed material as close as possible. This is also the conceptual root of why "cache misses," memory-bound performance, and things like array-of-structs vs. struct-of-arrays layout choices matter in real, performance-sensitive code.

### Pipelining and close
Briefly introduce pipelining as an assembly-line idea applied to the fetch-decode-execute cycle: instead of finishing instruction 1 completely before starting instruction 2, the CPU starts fetching instruction 2 while instruction 1 is still being decoded, and so on — multiple instructions in flight simultaneously, at different stages, which dramatically increases throughput without needing a faster clock. Note this is a major reason modern CPU speed gains over the last two decades have come less from raw clock-rate increases (which hit physical/thermal limits) and more from parallelism — pipelining, multiple cores, and related tricks. Close by tying the whole talk together top to bottom: bits are represented physically, gates combine bits into arithmetic, the fetch-decode-execute cycle turns arithmetic into running programs, the memory hierarchy makes running programs fast in practice, and pipelining squeezes more work out of the same clock — five layers, one continuous machine.

## Key Takeaway
A running program is, at every layer, still just electricity moving through switches interpreting patterns of bits — and the reason software performs the way it does (fast, slow, why overflow happens, why cache-friendly code matters) traces directly back to these hardware fundamentals.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.004 — Computation Structures](https://ocw.mit.edu/courses/6-004-computation-structures-spring-2017/pages/syllabus/)
