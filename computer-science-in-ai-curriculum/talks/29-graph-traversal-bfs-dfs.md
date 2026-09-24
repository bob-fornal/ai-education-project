# Graph Traversal: BFS & DFS

*This talk was split out from the general "Algorithms" backbone talk (see `03-algorithms.md`) — graph traversal is foundational enough, and has enough distinct applications, to merit its own dedicated session.*

## The One-Sentence Pitch
Breadth-first search and depth-first search are the two fundamental ways to explore a graph, and knowing which one to reach for — level-by-level with a queue, or deep-then-backtrack with a stack — unlocks a surprising range of everyday problems.

## Audience & Prerequisites
For learners who understand basic graphs (nodes and edges) and the difference between a queue and a stack; no prior traversal-algorithm experience required.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: two ways to explore a maze |
| 0:05–0:13 | Graph representations: adjacency list vs. matrix |
| 0:13–0:25 | Breadth-First Search mechanically |
| 0:25–0:40 | Depth-First Search mechanically and its applications |
| 0:40–0:47 | Decision rule: which one, and when |
| 0:47–0:50 | Key takeaway and close |

### Hook: two ways to explore a maze
Open with a simple mental image: dropped into a maze with no map, there are two natural exploration strategies — check everywhere one step away first, then everywhere two steps away, and so on outward (breadth-first), or commit to one path and follow it as far as it goes before backtracking to try another (depth-first). Both strategies are guaranteed to eventually find everything reachable, but they explore in a fundamentally different order — and that difference in order is exactly what makes each one suited to different problems, which is the throughline for the whole talk.

### Graph representations: adjacency list vs. matrix
Before traversal itself, cover how graphs are actually stored in memory, since this choice affects performance. Adjacency matrix: an n-by-n grid where cell (i, j) indicates whether an edge exists between node i and node j — checking if two specific nodes are connected is instant (one lookup), but the matrix takes O(n²) space regardless of how many edges actually exist. Adjacency list: each node keeps a list of just its actual neighbors — space usage is proportional to the actual number of edges, which is far smaller for sparse graphs (graphs where most node pairs aren't connected). Make the case concretely: a social network with a billion users but where each user has a few hundred friends is extremely sparse — an adjacency matrix would require space for a billion-squared possible edges, wildly wasteful, while an adjacency list only stores the edges that actually exist. This is why adjacency lists are the default choice for most real-world graphs, and matrices are reserved for dense graphs or when instant edge-existence checks matter more than memory.

### Breadth-First Search mechanically
Walk through BFS mechanically: start at a source node, put it in a queue, then repeatedly dequeue a node, look at all its unvisited neighbors, mark them visited and enqueue them. Because a queue is first-in-first-out, this naturally processes nodes in order of their distance from the source — everything at distance 1 gets enqueued and processed before anything at distance 2 gets discovered, since distance-2 nodes are only reachable through distance-1 nodes. Land the key insight explicitly: this is exactly why BFS finds shortest paths in an unweighted graph — since every edge "costs" the same single step, exploring in queue order is identical to exploring in order of increasing distance, so the first time you reach any node is guaranteed to be via a shortest path to it. Give a concrete use case: finding the shortest sequence of friend-connections between two people on a social network ("degrees of separation") is a textbook BFS application.

### Depth-First Search mechanically and its applications
Walk through DFS mechanically: start at a source node, and instead of exploring all neighbors immediately, pick one neighbor and recurse into it fully — going as deep as possible — only backtracking to try a different neighbor once a path dead-ends (visits a node with no unvisited neighbors left). This can be implemented with explicit recursion or with an explicit stack, and note that recursion is really just an implicit stack (the call stack) doing the same bookkeeping. Cover DFS's two signature applications with real depth. Topological sorting: for a graph of tasks with dependencies (do X before Y), running DFS and recording nodes in the order they finish (not the order they're first visited) produces a valid ordering where every dependency comes before what depends on it — this is exactly how build systems and course-prerequisite planners determine a valid execution order. Cycle detection: while running DFS, if you ever encounter an edge pointing back to a node that is still "in progress" further up the current recursive call chain (a "back edge"), that is proof a cycle exists — this is how systems detect circular dependencies (e.g., package A depends on B which depends on A). Mention connected components as a third natural application: running DFS (or BFS) repeatedly from any unvisited node, and counting how many separate runs it takes to cover the whole graph, tells you how many disconnected pieces the graph has — useful for anything from network reachability to image segmentation (flood fill is DFS/BFS in disguise).

### Decision rule: which one, and when
Give the audience a clear, memorable rule for choosing between them. Reach for BFS when the question involves shortest paths in an unweighted graph, or anything phrased as "level by level" or "minimum number of steps" — the queue-based level-order exploration is built for exactly this. Reach for DFS when the question is about "does a path exist at all" (not the shortest one), ordering things with dependencies (topological sort), detecting cycles, or exploring structural properties like connected components — DFS's willingness to commit deep before backtracking makes it naturally suited to these structural questions rather than distance questions. Note that both run in the same overall time complexity for a full traversal (proportional to the number of nodes plus edges), so the choice is about which one's exploration order actually answers your question, not about raw speed.

### Key takeaway and close
Recap the throughline: BFS and DFS explore the exact same graph in fundamentally different orders — level-by-level via a queue versus deep-then-backtrack via a stack — and that difference in order, not implementation difficulty, is what determines which one solves your specific problem.

## Key Takeaway
BFS's queue-driven level-order exploration makes it the right tool for shortest-path and "minimum steps" questions in unweighted graphs, while DFS's stack-driven depth-first exploration makes it the right tool for ordering, cycle-detection, and structural questions — knowing which order you need tells you which algorithm to use.

## Go Deeper
- Backbone home: `03-algorithms.md` in this folder · related: `28-dijkstras-algorithm.md` (the weighted generalization of BFS's shortest-path idea)
- MIT OCW: [6.006 — Introduction to Algorithms](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/pages/syllabus/) (explicitly covers "Graph Search: BFS, DFS, and Shortest Paths")
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) and [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md) also cover graph traversal in their data structures/algorithms coursework
