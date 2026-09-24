# Theory of Computation & Complexity

**Backbone Course #5** · **Duration:** 50 minutes

## The One-Sentence Pitch
Some questions about computation have provably certain answers — including hard limits on what any computer, no matter how powerful, can ever be made to do.

## Audience & Prerequisites
This talk is for learners with basic programming experience and some exposure to logic or proof (the discrete math talk is helpful but not required); no prior automata theory is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: not every problem yields to "just code harder" |
| 0:05–0:15 | Finite automata and regular languages |
| 0:15–0:25 | Turing machines and the Church-Turing thesis |
| 0:25–0:37 | Decidability and the halting problem |
| 0:37–0:46 | P and NP revisited |
| 0:46–0:50 | Close |

### Hook: not every problem yields to "just code harder"
Pose a question that sounds like it should just require more effort: "can I write a program that reads any other program's source code and tells me, with certainty, whether it will eventually finish or run forever?" Most beginners assume the answer is "yes, just simulate it and watch," and the goal of this talk is to show that the answer is a hard, proven no — not "no one has been clever enough yet," but "provably, permanently no." This talk is about the boundary of computation itself: what simple machines can and can't do, what a maximally general computer can and can't do, and why that boundary matters for anyone writing software today (compilers, security tools, and static analyzers all bump into it).

### Finite automata and regular languages
A finite automaton is about the simplest possible model of computation: a fixed, finite set of states, a set of transition rules based on the current input symbol, and a decision (accept/reject) at the end — no memory beyond "which state am I currently in." Use pattern matching as the intuitive frame: a finite automaton can recognize whether a string matches a simple pattern (like a valid email's basic shape, or "contains an even number of 1s"), and this is exactly the machinery underneath regular expressions used daily in text search and validation. Then show the limit concretely: a finite automaton cannot recognize "balanced parentheses" (matching open and close brackets of arbitrary nesting depth), because doing so requires remembering *how many* opens are still unmatched — an unbounded count — and a finite automaton has no memory for that, only a fixed number of states. This is the first proof-shaped limit of the talk: not every well-defined pattern-recognition task can be done by the simplest machine, no matter how many states you add, because the *number of states needed* would have to be infinite.

### Turing machines and the Church-Turing thesis
A Turing machine extends the finite automaton idea with an unbounded tape it can read, write, and move along — giving it unlimited memory, unlike a finite automaton — and this small addition is enough to make it as powerful as any computer that has ever been built. The Church-Turing thesis is the (unprovable, but universally accepted) claim that anything intuitively "computable" by any reasonable method — pen and paper, a modern laptop, a hypothetical future device — can be computed by some Turing machine; it defines computation itself, rather than describing one particular technology. Make the point vivid: your phone, a 1970s mainframe, and a Turing machine drawn on a whiteboard are all, in principle, equally powerful — differing only in speed and memory, never in which *problems* they can ultimately solve. This matters because it means "is this problem computable at all" is a well-defined, technology-independent question, which is exactly what the next segment answers for one famous case.

### Decidability and the halting problem
A problem is decidable if there exists some algorithm (Turing machine) that always halts and correctly answers yes or no for every input; the halting problem — "given a program and an input, will it eventually halt?" — is the canonical example of an undecidable problem, meaning no such algorithm can ever exist, for any input, ever. Sketch the diagonalization proof intuition informally without full rigor: assume, for contradiction, a magic `willHalt(program, input)` function exists; then construct a devious program that calls `willHalt` on *itself* and does the opposite of whatever it predicts (loops forever if predicted to halt, halts if predicted to loop) — this creates a direct contradiction no matter what `willHalt` outputs, so no such function can exist. Connect this to something tangible: this is exactly why no antivirus or compiler can *perfectly* detect all infinite loops or all malicious behavior in arbitrary code — they can catch many real cases with heuristics, but a general, always-correct solution is mathematically impossible, not just currently unbuilt. This is the sharpest example in the whole talk of "not yet solved" versus "proven impossible to solve" — a distinction most working programmers never get to see explicitly.

### P and NP revisited
Recall from the algorithms talk that P is "solvable in polynomial time" and NP is "a proposed solution can be verified in polynomial time"; here, add the complexity-theory framing explicitly: every problem in P is trivially also in NP (if you can solve it fast, you can certainly verify a solution fast), but whether the reverse holds — whether every efficiently-verifiable problem is also efficiently-solvable — is the P vs NP question, open since it was formalized in 1971. Note that thousands of important practical problems (scheduling, packing, certain optimization and logistics problems) are known to be NP-complete — meaning if *any* one of them had a fast algorithm, *all* of them would, via clever reductions between them — which is why so much of applied CS is about clever approximations and heuristics rather than exact fast solutions. Frame the stakes plainly: this single unsolved question has direct consequences for cryptography (much of it relies on some problems being hard to reverse) and for whether entire categories of optimization problems industries currently work around will ever have a genuinely fast, exact solution.

### Close
Trace the arc of the talk: finite automata showed a machine can be too weak for certain tasks no matter how you tune it, Turing machines and the Church-Turing thesis gave us a technology-independent definition of "computable" at all, the halting problem showed some computable-sounding questions are provably undecidable, and P vs NP showed even among decidable problems, "fast" and "slow" may be a permanent, unbridgeable divide for entire problem classes. The unifying theme: computer science has actual, provable limits, not just current engineering gaps — and knowing the difference is part of thinking like a computer scientist rather than just a programmer.

## Key Takeaway
Some limits on computation are not "we haven't figured it out yet" — they are proven, permanent boundaries (the halting problem is undecidable; P vs NP remains unresolved but deeply consequential), and recognizing which kind of limit you're up against changes how you approach a hard problem.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [18.404J — Theory of Computation](https://ocw.mit.edu/courses/18-404j-theory-of-computation-fall-2020/pages/syllabus/)
