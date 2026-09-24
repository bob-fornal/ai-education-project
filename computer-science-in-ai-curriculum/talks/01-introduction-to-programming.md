# Introduction to Programming

**Backbone Course #1** · **Duration:** 50 minutes

## The One-Sentence Pitch
Programming is not primarily about learning a syntax — it's about learning to break a fuzzy, human problem into a sequence of precise, unambiguous steps a machine can follow.

## Audience & Prerequisites
This talk is for complete beginners to CS — no prior coding experience is assumed, only comfort with basic logical reasoning (following an "if this, then that" instruction).

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: what programming actually is |
| 0:05–0:15 | Variables and primitive types |
| 0:15–0:27 | Control flow: conditionals and loops |
| 0:27–0:37 | Functions and procedural abstraction |
| 0:37–0:44 | A first taste of data structures |
| 0:44–0:50 | Worked example and close |

### Hook: what programming actually is
Open with a deliberately non-technical example: giving someone directions to make a peanut butter sandwich, and watching how many "obvious" steps get skipped (open the jar, pick up the knife, etc.). Computers have zero common sense — they will do exactly, and only, what they are told, in exactly the order they are told it. This reframes programming away from "memorizing a language" and toward "problem decomposition" — turning a vague goal into an ordered list of unambiguous, mechanical steps. Every topic in the rest of the talk is really just vocabulary for expressing that decomposition more precisely.

### Variables and primitive types
A variable is a named location that holds a value, and giving it a name is what lets us refer to "that data" without knowing its exact value in advance — this is the seed of abstraction. Introduce primitive types — integers, floating-point numbers, booleans, characters/strings — as the basic units of data a program manipulates, and note that a type is really a promise about what operations are valid and how the bits should be interpreted. Use a concrete example: `age = 30` versus `name = "Ada"` versus `isEnrolled = true`, and point out that mixing types carelessly (adding a number to text) is where a huge fraction of early bugs come from. The core idea: a variable plus a type is how we give a fuzzy real-world quantity a precise, machine-checkable representation.

### Control flow: conditionals and loops
Conditionals (`if`/`else`) let a program make a decision — a fork in the road based on a boolean condition — and this is the direct mechanization of "if this, then that" reasoning from the hook. Loops (`for`/`while`) let a program repeat a step without the programmer writing it out N times, which matters because most real problems involve processing many items, not one. Walk through a simple example live: checking whether a number is even (conditional) versus summing all numbers from 1 to 100 (loop), and show how a loop turns a 100-line manual list into 3 lines. Emphasize that control flow is what turns a straight-line script into something that can actually respond to different inputs and different amounts of data — this is where "precise steps" starts to look like real problem-solving.

### Functions and procedural abstraction
A function is a named, reusable block of steps that takes input and (usually) produces output — it's the same abstraction idea as a variable, but applied to *behavior* instead of *data*. Introduce the idea of procedural abstraction: once a function like `isPrime(n)` is written and tested, everything else in the program can use it without caring how it works internally, only what it promises to do. This is the single most important scaling mechanism in programming — it's how a 10-line toy program and a 10-million-line production system are built from the same basic idea, just composed more deeply. Give a small example: a `sandwich()` function that internally calls `getBread()`, `addPeanutButter()`, `addJelly()` — decomposition made literal in code.

### A first taste of data structures
Introduce arrays/lists as the natural next step once we have more than one piece of data to track — instead of `student1`, `student2`, `student3`, we have `students = [...]`, and now a loop can process all of them uniformly. Show how this connects directly back to loops: "for each student in students, check if they passed" is both a plain-English sentence and a working piece of code. Mention briefly that this is just the beginning — different ways of organizing data (which the next talk covers in depth) have very different performance tradeoffs, but the deep principle is already visible: shape data to match the operations you need to perform on it.

### Worked example and close
Bring it all together with one small live-coded problem: given a list of exam scores, compute the average and report how many students passed (score >= 60). Show the decomposition explicitly: a variable for the threshold, a loop over the list, a conditional inside the loop, a running total, and optionally a function `didPass(score)` to make the logic reusable and readable. Close by returning to the hook — every piece of this program is just a precise restatement of a fuzzy instruction ("tell me how the class did"), and that translation skill, not syntax recall, is what separates someone who can program from someone who has merely memorized keywords.

## Key Takeaway
Syntax is easily looked up; the durable skill is decomposition — turning an ambiguous problem into a sequence of variables, decisions, repetitions, and reusable steps that a machine can execute exactly.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.0001 — Introduction to Computer Science and Programming in Python](https://ocw.mit.edu/courses/6-0001-introduction-to-computer-science-and-programming-in-python-fall-2016/pages/syllabus/)
- Harvard PLL: [CS50: Introduction to Computer Science](https://pll.harvard.edu/course/cs50-introduction-computer-science)
