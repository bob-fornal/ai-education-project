
# Artificial Intelligence

**Backbone Course #17** · **Duration:** 50 minutes

## The One-Sentence Pitch
Before AI meant "models that learn from data," it meant machines that searched through possibilities and reasoned over hand-built facts and rules — and the search algorithm at the center of that era, A*, still runs in your GPS and every game you've played.

## Audience & Prerequisites
For learners who have seen basic graphs, arrays, and recursion; no prior AI or machine learning background is assumed, and this talk deliberately stays on the classical/symbolic side of AI history.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | What "AI" has meant — two eras |
| 0:05–0:15 | Search as the unifying idea of classical AI |
| 0:15–0:35 | A* Search — Dijkstra plus a heuristic |
| 0:35–0:45 | Knowledge representation and logic |
| 0:45–0:50 | Bridge to learning-based AI and close |

### What "AI" has meant — two eras
"Artificial intelligence" is not one technique; it's a decades-long goal pursued with very different tools. From the 1960s through the 1980s, AI mostly meant symbolic AI: programs that searched through explicitly defined possibilities and reasoned using hand-coded facts and logical rules written by human experts. Today, "AI" more often means learning-based AI: systems that infer their own patterns from large datasets, which is the subject of the Machine Learning and Deep Learning talks elsewhere in this series. This talk is entirely about the classical/foundational side — the ideas that gave AI its name and its first working systems, and that still power huge amounts of real software today (pathfinding, planning, scheduling, theorem proving). Understanding this era also explains why modern AI looks the way it does — it grew out of, and in many ways reacted against, these earlier approaches.

### Search as the unifying idea of classical AI
Almost every classical AI problem can be reframed as searching through a state space: a set of possible situations (states), a set of moves that get you from one state to another (actions), a starting state, and a goal test. Playing chess, solving a sliding-puzzle, planning a robot's path through a room, and routing a delivery truck are all, structurally, the same problem — you are exploring a branching space of possibilities looking for a sequence of actions that reaches a goal. The art of classical AI is choosing which parts of that space to explore, because the space is almost always too big to search exhaustively. Two familiar building blocks handle "blind" exploration: breadth-first search expands the closest states first and guarantees the shortest path in an unweighted graph, while depth-first search plunges down one branch before backtracking, using less memory but no such guarantee. Both are "uninformed" — they have no sense of which direction is promising — which sets up the motivation for something smarter.

### A* Search — Dijkstra plus a heuristic
A* is the single most important algorithm to come out of classical AI's search tradition, and it is explicitly an extension of Dijkstra's algorithm. Dijkstra's algorithm finds the shortest path from a start node by always expanding the frontier node with the lowest known cost-so-far, `g(n)`; it is correct and complete but explores in every direction equally, wasting effort exploring toward nowhere useful. A* adds one idea: for every candidate node, also estimate the remaining distance to the goal using a heuristic function `h(n)`, and always expand the node that minimizes `f(n) = g(n) + h(n)` — cost so far, plus estimated cost to go. That estimate lets A* aim its search toward the goal instead of spreading out uniformly, which is why it explores dramatically fewer nodes than Dijkstra's algorithm on the same problem while still finding the shortest path. The catch is that the heuristic must be admissible — it must never overestimate the true remaining cost — because an overestimate can cause A* to prematurely discard a path that was actually optimal; a classic admissible heuristic for grid movement is straight-line ("as the crow flies") distance to the goal, since you can never travel a shorter path than a straight line. This single algorithm is the workhorse behind two things people use daily: non-player-character pathfinding in essentially every video game (characters navigating around obstacles toward a target), and the route-finding core of GPS and mapping software (finding the shortest or fastest path through a road network). It's worth pausing on this concretely: A* with an admissible heuristic is provably guaranteed to return the optimal (shortest/cheapest) path, not just a good-enough one — that guarantee, combined with real speed, is exactly why it displaced plain Dijkstra in so many production systems.

### Knowledge representation and logic
The other half of classical AI is not about searching through moves but about representing what a machine knows and letting it derive new facts logically. The idea is to encode facts and rules in a formal language precise enough that a machine can manipulate them correctly — the classic tool for this is first-order logic, which lets you state facts ("Socrates is a man"), general rules ("all men are mortal"), and then mechanically derive new conclusions ("Socrates is mortal") using formal inference rules rather than guesswork. Systems built this way, often called expert systems, encoded a human expert's rules for a narrow domain (medical diagnosis, equipment troubleshooting) and could then answer questions or justify a conclusion by showing the chain of rules that led to it — a kind of explainability that's genuinely hard to get from modern learned models. The strength of this approach is that it's exact and explainable; the weakness, which became the era's central lesson, is that it's brittle: encoding "common sense" knowledge by hand turned out to be enormous, tedious, and never quite complete enough to handle the real world's exceptions.

### Bridge to learning-based AI and close
That brittleness of hand-coded rules is precisely the problem modern, learning-based AI set out to solve. Instead of a human writing down every rule, a learning system is shown many examples and infers the underlying pattern itself — trading hand-built precision and explainability for flexibility and the ability to handle messy, real-world data that no expert could fully enumerate rules for. That shift — from encoded knowledge to learned patterns — is exactly where the Machine Learning and Deep Learning talks in this series pick up.

## Key Takeaway
Classical AI reframed hard problems as search through a state space or as logical inference over encoded facts — and A*, "Dijkstra's algorithm plus an admissible heuristic," is the concrete algorithm from that era still routing your GPS and your game characters today.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.034 — Artificial Intelligence](https://ocw.mit.edu/courses/6-034-artificial-intelligence-fall-2010/pages/syllabus/)
- Harvard PLL: [CS50's Introduction to Artificial Intelligence with Python](https://pll.harvard.edu/course/cs50s-introduction-artificial-intelligence-python)
- See also in this folder: `28-dijkstras-algorithm.md`, `29-graph-traversal-bfs-dfs.md`
