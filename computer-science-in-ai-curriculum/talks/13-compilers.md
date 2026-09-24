# Compilers

**Backbone Course #13** · **Duration:** 50 minutes

## The One-Sentence Pitch
A compiler's job is to take human-readable source code through a pipeline of well-defined stages — understanding its structure, checking its meaning, and reshaping it — until it becomes something a machine can execute correctly and efficiently.

## Audience & Prerequisites
This talk is for learners comfortable writing basic programs and reasoning about expressions like `x = 2 + 3 * y`; no prior compiler or formal language theory is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: your code is not what the machine runs |
| 0:05–0:15 | The pipeline: lexing through code generation |
| 0:15–0:24 | Abstract syntax trees |
| 0:24–0:33 | Why optimization matters |
| 0:33–0:43 | Interpreters vs. compilers |
| 0:43–0:50 | Worked example and close |

### Hook: your code is not what the machine runs
Open with a simple but underappreciated fact: the text you type into an editor — with its indentation, comments, and variable names like `totalScore` — bears almost no resemblance to what the processor actually executes, which is raw numeric instructions with no names, no comments, and no forgiving whitespace. Something has to bridge that enormous gap, transforming human-friendly text into machine-friendly instructions while preserving the program's exact intended meaning. That something is a compiler (or an interpreter, its close cousin covered later), and today's talk walks through the stages that bridge does the work in.

### The pipeline: lexing through code generation
Follow one expression, `x = 2 + 3 * y`, through each stage. Lexing (tokenizing) breaks the raw character stream into meaningful chunks — `x`, `=`, `2`, `+`, `3`, `*`, `y` — the same way you mentally break a sentence into words before understanding it. Parsing takes those tokens and checks whether they form a valid structure according to the language's grammar, building a tree that captures how the pieces relate (crucially, respecting that `*` binds tighter than `+`, so `3 * y` is grouped before adding `2`). Semantic analysis then checks whether the structurally valid code actually *makes sense* — is `y` even defined at this point, are the types compatible for addition — catching errors that are grammatically fine but meaningless. From there the compiler generates an intermediate representation, a lower-level but still portable form of the computation, and finally code generation converts that down into the actual machine instructions the processor will run — for our example, something like "load y, multiply by 3, load 2, add, store into x."

### Abstract syntax trees
The abstract syntax tree (AST) is the structural representation parsing produces — a tree where each node is an operation and its children are the things that operation applies to, discarding the incidental details of the original text (exact spacing, parentheses that were just there for grouping) while preserving everything about actual meaning. For `x = 2 + 3 * y`, the tree naturally has an assignment node at the root, with `x` on one side and a `+` node on the other, and that `+` node has `2` as one child and a `*` node (with `3` and `y`) as the other — the tree shape itself encodes the order of operations, which is exactly why parsing needs to happen before anything else can proceed correctly. Every later stage — semantic analysis, optimization, code generation — operates on this tree rather than on the original text, because a tree is something a program can walk, inspect, and rewrite in a structured way that raw text never allows.

### Why optimization matters
Two programs can compute the exact same correct answer while running at wildly different speeds, and optimization is the set of transformations a compiler applies to the intermediate representation to close that gap without changing what the program computes. A simple example: if part of an expression never changes inside a loop (like recomputing `3 * y` on every iteration when `y` never changes), a compiler can notice this and hoist that computation outside the loop, computing it once instead of a million times. Other classic optimizations include eliminating dead code that can never execute, and simplifying constant expressions (`2 + 3` becomes `5` at compile time instead of being recomputed every run). The point to land: correctness and performance are separate concerns handled at separate stages — semantic analysis worries about "is this right," optimization worries about "can this be made faster without becoming wrong."

### Interpreters vs. compilers
A compiler translates source code into a different, typically lower-level form ahead of time, producing an executable that can be run later, independently, without the compiler present — think of it as translating an entire book into another language before anyone reads it. An interpreter instead reads and executes source code directly, statement by statement, translating and running each piece on the fly — like a live interpreter translating a speech sentence by sentence as it's being given. Both are solving the exact same underlying problem — bridging human-written code and machine execution — they just differ in *when* that translation happens and whether a standalone translated artifact is produced. Mention that this isn't strictly binary in practice — many modern language runtimes blend both approaches, interpreting code initially and compiling the "hot," frequently run parts on the fly — but the conceptual distinction between ahead-of-time translation and on-the-fly execution is the important takeaway.

### Worked example and close
Recap the full journey of `x = 2 + 3 * y`: lexing turns it into tokens, parsing builds the AST that correctly captures precedence, semantic analysis confirms `y` is defined and the types line up, optimization might simplify or reorder subexpressions where safe, and code generation emits the final machine instructions — and every single stage exists because the previous one produced something too raw or ambiguous for the next one to work with directly. Close by connecting back to the hook: what looked like a two-line arithmetic statement actually passes through half a dozen distinct, well-understood transformations before a single instruction ever reaches the processor, and that entire pipeline is invisible every single time you run a program.

## Key Takeaway
A compiler's pipeline — lexing, parsing into an AST, semantic checking, optimization, and code generation — exists because each stage needs the previous stage's output in a more structured, more verified form than raw text, and this same pipeline shape reappears anywhere a program needs to understand and transform other programs.

## Go Deeper
- [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.035 — Computer Language Engineering](https://ocw.mit.edu/courses/6-035-computer-language-engineering-spring-2010/pages/syllabus/)
