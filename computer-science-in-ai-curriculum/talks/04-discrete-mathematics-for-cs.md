# Discrete Mathematics for Computer Science

**Backbone Course #4** · **Duration:** 50 minutes

## The One-Sentence Pitch
Discrete math is the vocabulary and proof toolkit that lets computer scientists state claims about programs precisely and know, with certainty rather than testing alone, whether those claims are true.

## Audience & Prerequisites
This talk is for learners with basic programming experience and comfort with high-school algebra; no prior formal logic or proof-writing background is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: why testing isn't proof |
| 0:05–0:15 | Propositional and predicate logic |
| 0:15–0:30 | Proof techniques, with induction worked live |
| 0:30–0:40 | Sets, relations, and functions |
| 0:40–0:47 | A taste of combinatorics and graph theory |
| 0:47–0:50 | Close |

### Hook: why testing isn't proof
Open with a pointed question: "if I test my function on 1,000 inputs and it works every time, do I know it's correct?" The honest answer is no — testing can reveal the presence of bugs but never their absence, because it only checks the cases you happened to try. Discrete math exists to close that gap: it gives precise language for stating exactly what a program should do, and proof techniques for establishing that it does so for *every* possible input, not just the sampled ones. This reframes the whole subject away from "abstract math for its own sake" and toward "the tools that let you reason about infinite cases using a finite argument."

### Propositional and predicate logic
Propositional logic works with statements that are simply true or false, combined with AND, OR, NOT, and IMPLIES — and this is literally the logic embedded in every `if` statement and boolean expression already written in code. Predicate logic extends this with quantifiers — "for all" (∀) and "there exists" (∃) — which let us make claims about entire collections at once, like "for all inputs n, this function returns a non-negative number." Show the translation both ways: the English sentence "every sorted list has no inversions" becomes ∀ pairs (i,j) where i < j, list[i] ≤ list[j] — and conversely, being able to read that notation is what lets you understand precise specifications, textbooks, and papers. This logic is also exactly what powers search engines' boolean queries and database WHERE clauses, so it's not abstract — it's already running underneath tools used daily.

### Proof techniques, with induction worked live
Introduce direct proof (assume the hypothesis, chain implications to the conclusion) and proof by contradiction (assume the claim is false, derive an impossibility, conclude the claim must be true) as the two most intuitive proof styles, each mirroring reasoning already used informally. Then spend the bulk of this segment on mathematical induction, since it's the technique most directly useful for reasoning about code (loops, recursion): state the two-step structure — prove a base case, then prove that if the claim holds for n, it holds for n+1 — and explain why this is enough to cover all natural numbers, like an infinite row of dominoes where knocking over the first guarantees all the rest fall. Work a real mini-example live: prove that 1 + 2 + ... + n = n(n+1)/2, showing the base case n=1 checks out, then assuming it holds for n and algebraically deriving that it must then hold for n+1. Explicitly connect this back to code: proving a loop invariant (a condition that stays true every iteration) uses exactly this induction structure, which is how programmers formally argue a loop is correct rather than just "probably fine."

### Sets, relations, and functions
A set is simply an unordered collection of distinct items, and set operations — union, intersection, difference — show up constantly, from SQL joins to deduplicating a list. A relation is a set of ordered pairs connecting elements of two sets (e.g., "is a friend of," "is enrolled in"), and a function is a special, restricted kind of relation where every input maps to exactly one output — which is precisely the mathematical definition underlying every function written in code. Point out that "the domain and range of a function" isn't just textbook language — it's the same idea as a function's parameter types and return type, and thinking about whether a function is one-to-one or has an inverse maps directly onto whether an operation (like encoding/decoding) can be reliably undone. This vocabulary is the connective tissue between "informal function in code" and "formal function in math," and it's what lets the two fields borrow techniques from each other.

### A taste of combinatorics and graph theory
Combinatorics is the mathematics of counting carefully — how many ways can you arrange, choose, or order things — and it matters in CS because algorithm analysis often boils down to "how many cases/paths/subsets are there," which directly determines whether an approach is fast or exponentially slow. Give one crisp example: the number of subsets of an n-element set is 2^n, which is exactly why brute-force approaches that try "every subset" become impossibly slow past a few dozen items — a concrete, countable reason exponential algorithms are avoided. Introduce a graph simply as a set of nodes (vertices) connected by edges — modeling anything from social networks to road maps to web page links — and note that this innocuous-looking structure is the foundation for the shortest-path and traversal algorithms covered in dedicated talks later in this series. The goal here is just fluency with the vocabulary (vertex, edge, directed vs. undirected, path, cycle) so those later talks can move quickly.

### Close
Tie the four pieces together: logic gives precise statements, proof techniques (especially induction) let us verify those statements hold universally, sets/relations/functions give the shared vocabulary underlying both math and code, and combinatorics/graphs give us tools for counting and modeling structure. None of this replaces testing — it complements it, giving certainty testing alone cannot.

## Key Takeaway
Discrete math is not decoration for CS — induction in particular is the formal version of "prove my loop works for every iteration," and that habit of precise, universal reasoning is what separates code that's been tested from code that's been proven correct.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.1200J — Mathematics for Computer Science](https://ocw.mit.edu/courses/6-1200j-mathematics-for-computer-science-spring-2024/pages/syllabus/)
