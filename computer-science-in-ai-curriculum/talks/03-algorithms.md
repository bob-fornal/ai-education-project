# Algorithms: Analysis and Design Paradigms

**Backbone Course #3** · **Duration:** 50 minutes

## The One-Sentence Pitch
Algorithms are best understood not as a list of tricks to memorize but as a small set of reusable design paradigms, evaluated by how their running time grows as input size grows.

## Audience & Prerequisites
This talk is for learners comfortable with basic programming and data structures (arrays, recursion, and function calls); no prior exposure to complexity theory is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: why "how fast" needs a better definition |
| 0:05–0:15 | Asymptotic analysis and Big-O |
| 0:15–0:25 | Divide-and-conquer |
| 0:25–0:34 | Dynamic programming |
| 0:34–0:41 | Greedy algorithms |
| 0:41–0:47 | P vs NP: an accessible taste |
| 0:47–0:50 | Close |

### Hook: why "how fast" needs a better definition
Start with a deliberately misleading question: "which is faster, algorithm A or algorithm B?" and show two functions timed on a small input where the "slower-looking" one wins — then rerun conceptually on a much larger input where the ranking flips. A stopwatch measurement depends on the machine, the language, and the input size of the moment, so it can't answer "which algorithm scales better," which is usually the question that actually matters in practice. This motivates the need for a machine-independent way to talk about growth rate — the subject of the next segment — before getting into the paradigms that produce fast or slow algorithms in the first place.

### Asymptotic analysis and Big-O
Big-O notation describes how an algorithm's running time grows as input size (n) grows, ignoring constant factors and lower-order terms — O(n) means "roughly doubles when input doubles," O(n²) means "roughly quadruples," O(log n) means "barely grows at all." Walk through a simple side-by-side: linear search is O(n) because it may scan every element, while binary search on sorted data is O(log n) because each comparison halves the remaining search space — the same intuition as the BST lookups from the data structures talk. Emphasize what Big-O deliberately throws away (constant factors, hardware speed, implementation language) in order to isolate the one thing that matters most as problems get large: the shape of the growth curve. This vocabulary — O(1), O(log n), O(n), O(n log n), O(n²), O(2^n) — is the shared language the rest of the talk, and most of CS, uses to compare algorithms honestly.

### Divide-and-conquer
Divide-and-conquer solves a problem by splitting it into smaller subproblems of the same type, solving each recursively, and combining the results — the classic structural pattern behind merge sort. Walk through merge sort conceptually: split the list in half, recursively sort each half, then merge two sorted halves together in one linear pass — and note that this split-solve-combine shape is what produces the O(n log n) running time (log n levels of splitting, O(n) work to merge at each level). The key insight to land: divide-and-conquer works well when a problem's subproblems are independent — solving the left half tells you nothing extra about the right half — which is exactly the property that lets the recursive calls run without needing to share information.

### Dynamic programming
Dynamic programming (DP) applies when a problem has overlapping subproblems — unlike divide-and-conquer, the same smaller subproblem gets asked more than once, so naive recursion wastes enormous time recomputing it. Use the Fibonacci example: naive recursive `fib(n)` recomputes `fib(n-2)` many times over and blows up to exponential time, but storing each result the first time it's computed (memoization) collapses it to O(n). Extend briefly to a slightly richer example, the coin change problem (fewest coins to make a target amount) — showing that the "best answer for amount X" only needs the best answers for smaller amounts, which is the general DP pattern: define subproblems, find the recurrence relating them, and store answers so no subproblem is solved twice. The takeaway: DP is not a separate bag of tricks, it's divide-and-conquer's cousin, with a memory added specifically to avoid redundant work.

### Greedy algorithms
A greedy algorithm builds a solution step by step, always taking the locally best-looking choice at each step and never reconsidering it — which is fast and simple, but only produces a globally correct answer for certain problems. Use a coin-making example with a "nice" coin system (like US coins: quarters, dimes, nickels, pennies) where always picking the largest coin that fits works perfectly, then contrast with a constructed coin system where greedy fails to find the true minimum — making the risk tangible rather than abstract. Mention a simple scheduling example too: given a set of tasks with start/end times, greedily picking the task that finishes earliest at each step actually does provably maximize the number of tasks scheduled — so greedy isn't always wrong, it's just unreliable without a proof that the "locally best" choice can't hurt later. The lesson: greedy is the fastest and simplest paradigm when it works, but it demands justification, not just intuition, before trusting it.

### P vs NP: an accessible taste
Introduce the class P as "problems solvable in polynomial time" (roughly, the algorithms discussed so far) and NP as "problems where a proposed solution can be *checked* quickly, even if finding one from scratch might be much harder" — like a Sudoku puzzle: verifying a filled grid is fast, but solving a blank one can be very slow. State plainly that whether P equals NP — whether every quickly-checkable problem is also quickly-solvable — is one of the most famous open problems in mathematics and computer science, unsolved despite decades of effort and a $1M prize attached to it. The point for this audience isn't the proof machinery, it's the meaning: "we don't know a fast algorithm" is sometimes not a personal failure of cleverness, it may reflect a deep, currently-unproven structural limit on computation itself — a preview of the Theory of Computation talk later in this series.

Note for the audience: this talk deliberately stays at the level of paradigms and complexity classes rather than walking through specific famous algorithms in depth. Dijkstra's shortest-path algorithm, breadth-first/depth-first graph traversal, and sorting/searching algorithms (binary search, quicksort, mergesort) each get their own dedicated 50-minute talk elsewhere in this series (`28-dijkstras-algorithm.md`, `29-graph-traversal-bfs-dfs.md`, `30-sorting-and-searching-fundamentals.md`), where there is room to go deep on a single algorithm's mechanics and correctness proof. Treat this talk as the conceptual scaffolding those talks will hang on.

### Close
Recap the arc: Big-O gave us a shared vocabulary for growth rate, then three paradigms — divide-and-conquer, dynamic programming, greedy — gave us three different strategies for building algorithms, and P vs NP showed there's a hard ceiling we don't yet understand. Point forward explicitly to the dedicated algorithm talks in this series as where these paradigms get applied to real, named algorithms in full detail.

## Key Takeaway
Don't memorize algorithms one at a time — recognize which of the handful of design paradigms (divide-and-conquer, DP, greedy) a new problem resembles, and use Big-O to honestly compare the options before committing to one.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.046J — Design and Analysis of Algorithms](https://ocw.mit.edu/courses/6-046j-design-and-analysis-of-algorithms-spring-2015/pages/syllabus/)
