# Dijkstra's Algorithm & Shortest Paths

*This talk was split out from the general "Algorithms" backbone talk (see `03-algorithms.md`) because Dijkstra's Algorithm is significant enough — and used widely enough in the real world — to deserve its own full 50-minute treatment rather than a rushed mention.*

## The One-Sentence Pitch
Dijkstra's algorithm finds the cheapest path from a start node to every other node in a weighted graph by greedily and repeatedly locking in the closest unresolved node — and it powers everything from GPS routing to the internet's own backbone routing protocol.

## Audience & Prerequisites
For learners who already understand basic graphs (nodes and edges) and have seen a priority queue conceptually; no prior shortest-path or greedy-algorithm background required.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: the shortest-path problem is everywhere |
| 0:05–0:12 | Framing the problem precisely |
| 0:12–0:32 | The greedy relaxation mechanism, traced on an example |
| 0:32–0:40 | Why it breaks with negative weights |
| 0:40–0:47 | Real-world applications: GPS and OSPF |
| 0:47–0:50 | Key takeaway and close |

### Hook: the shortest-path problem is everywhere
Open by pointing out that "find the cheapest way to get from A to B" is one of the most commonly solved problems in computing, hiding behind features people use daily — a maps app picking a driving route, a network router deciding how to forward a packet, a logistics company routing delivery trucks. All of these reduce to the same abstract problem on a graph, and one algorithm, invented in 1956 by Edsger Dijkstra, is still the standard tool for solving it today.

### Framing the problem precisely
State the single-source shortest-path problem precisely: given a graph where edges have non-negative weights (costs, like distance or time) and a designated start node, find the cheapest total-weight path from the start to every other node in the graph. Emphasize "non-negative" as a load-bearing detail that will matter later. Contrast this with unweighted shortest paths (where BFS suffices because every edge costs the same) — here, edges have different costs, so the cheapest path isn't necessarily the one with the fewest hops, and that's exactly what makes the problem harder and Dijkstra's algorithm necessary.

### The greedy relaxation mechanism, traced on an example
Explain the algorithm's mechanism as a greedy process. Maintain a running "best known distance" for every node (start at 0, everything else at infinity), and a priority queue of nodes ordered by that distance. Repeatedly: pull the not-yet-finalized node with the smallest known distance out of the priority queue (this is the greedy choice — always expand the cheapest frontier node next), mark it finalized, then "relax" each of its neighbors — check if going through the just-finalized node offers a cheaper path than the neighbor's current best-known distance, and if so, update it. Trace this on a small concrete example described in words: five nodes, A through E, where A connects to B (weight 4) and C (weight 1); C connects to B (weight 2) and D (weight 5); B connects to D (weight 1); D connects to E (weight 3). Starting from A: A is finalized at distance 0. Its neighbors B (distance 4) and C (distance 1) get queued. C has the smaller distance, so C is finalized next at distance 1; relaxing C's neighbors, B's distance improves from 4 to 3 (1 + 2) and D gets set to 6 (1 + 5). B now has the smallest distance (3), so B is finalized next; relaxing B's neighbor D, we find 3 + 1 = 4, which beats D's current 6, so D updates to 4. D is finalized next at distance 4; relaxing its neighbor E gives 4 + 3 = 7. Finally E is finalized at distance 7. Walking through this trace makes visible exactly why the priority queue matters: it always hands you the correct next node to finalize, in cheapest-first order.

### Why it breaks with negative weights
Explain the core assumption the greedy approach relies on: once a node is finalized, its distance is treated as permanently correct and never revisited. This assumption depends entirely on edge weights being non-negative — because with only non-negative weights, no later path through unexplored nodes could ever end up cheaper than the greedy choice already made. Show why a negative edge weight breaks this: if some later, unexplored edge could subtract from a path's total cost, a node finalized "too early" might actually have had a cheaper true distance reachable through a path Dijkstra had already dismissed as more expensive — but Dijkstra never goes back to check, so it can produce a wrong answer. This is precisely why Bellman-Ford exists as the fallback algorithm for graphs with negative edge weights: it takes a slower, more exhaustive approach (repeatedly relaxing every edge in the graph, not just the greedy frontier) that remains correct even when negative weights are present, at the cost of worse time complexity.

### Real-world applications: GPS and OSPF
Give the applications genuine weight rather than a passing mention. GPS navigation systems model road networks as weighted graphs (weights representing driving time or distance) and run shortest-path computations essentially equivalent to Dijkstra's algorithm (often with practical optimizations layered on) to compute the fastest route in real time as traffic conditions change. On the internet's infrastructure side, OSPF (Open Shortest Path First) is a real, widely deployed routing protocol that literally runs Dijkstra's algorithm inside routers — each router builds a map of the network's link costs and uses Dijkstra's algorithm to compute the shortest path to every other router, which is how packets get efficiently routed across large networks without any centralized coordinator. Point out that this is a rare case of a 1956 algorithm still running, unmodified in its core logic, inside production infrastructure that carries a meaningful fraction of the world's internet traffic today.

### Key takeaway and close
Recap the mechanism — greedily finalize the cheapest known node, relax its neighbors, repeat — and the two caveats that matter most in practice: it requires non-negative weights (otherwise use Bellman-Ford), and it is not a museum piece — it is running right now inside GPS systems and internet routers.

## Key Takeaway
Dijkstra's algorithm's greedy "always expand the cheapest known frontier node next" strategy is provably correct for non-negative-weight graphs, and it isn't just a textbook exercise — it's the literal algorithm running inside GPS routing and the OSPF protocol that helps route internet traffic today.

## Go Deeper
- Backbone home: `03-algorithms.md` in this folder (the paradigm-level talk this was split from) · related: `29-graph-traversal-bfs-dfs.md`, `17-artificial-intelligence.md` (covers A*, which extends Dijkstra's with a heuristic)
- [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md), [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md), and [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md) all cover graph shortest-path algorithms as part of their algorithms coursework
- Source: [GeeksforGeeks — Dijkstra's Shortest Path Algorithm](https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/)
