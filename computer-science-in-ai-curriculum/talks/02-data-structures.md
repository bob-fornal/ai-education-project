# Data Structures

**Backbone Course #2** · **Duration:** 50 minutes

## The One-Sentence Pitch
Choosing a data structure is really choosing which operations on your data will be fast, which will be slow, and there is no structure that makes everything fast at once.

## Audience & Prerequisites
This talk is for learners who already have Introduction to Programming under their belt — comfort with variables, loops, and functions is assumed; no prior data structures knowledge is needed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: the tradeoff behind every structure |
| 0:05–0:14 | Arrays vs. linked lists |
| 0:14–0:22 | Stacks and queues |
| 0:22–0:32 | Trees and binary search trees |
| 0:32–0:41 | Hash tables |
| 0:41–0:50 | Synthesis and close |

### Hook: the tradeoff behind every structure
Frame the whole talk around one question a working programmer asks constantly: "how will this data be accessed, and how often?" A phone book you search by name a thousand times a day needs a very different organization than a to-do list you only ever add to and remove from one end. Data structures are not arbitrary trivia to memorize — each one is a deliberate answer to "make *this* operation fast, at the cost of making *that* operation slower." Every section that follows will name the operation each structure is optimized for, and the operation it sacrifices.

### Arrays vs. linked lists
An array stores elements in contiguous memory, so the computer can jump directly to element `i` using simple arithmetic — O(1) random access — but inserting or removing from the middle means shifting every subsequent element. A linked list stores each element in its own node with a pointer to the next node, so elements can live anywhere in memory; insertion/removal at a known point is O(1), but reaching element `i` means walking the list from the start — O(n). Use a concrete contrast: an array is like numbered parking spaces (instant lookup by number, but inserting a new spot means renumbering everyone after it); a linked list is like a scavenger hunt (each clue points to the next, easy to insert a new clue, but you can't skip ahead). The lesson: contiguous memory buys fast access, pointer-based memory buys fast insertion — pick based on which one your program does more.

### Stacks and queues
A stack is Last-In-First-Out (LIFO) — think of a stack of plates, you can only take from the top — and it maps directly onto real behavior like the "undo" button in an editor or the call stack tracking function calls in a running program. A queue is First-In-First-Out (FIFO) — think of a checkout line — and it maps onto task scheduling, print queues, or handling requests in the order they arrived. Both can be built on top of either arrays or linked lists, which is a good moment to reinforce that "data structure" (the interface: push/pop, enqueue/dequeue) and "implementation" (array vs. linked list underneath) are different layers. Ask the audience to spot which one they'd want for browser back-button history (stack) versus a customer service call queue (queue) to cement the intuition.

### Trees and binary search trees
A tree generalizes a linked list by letting each node have multiple children, which is the natural shape for hierarchical data — file systems, org charts, HTML/DOM structure. A binary search tree (BST) adds an ordering rule: every left descendant is smaller than a node, every right descendant is larger, which means a well-balanced BST supports search, insert, and delete in O(log n) — because each comparison eliminates roughly half the remaining tree, the same idea as binary search. Draw a small BST live and trace a lookup for a specific value, counting comparisons, to make the log n behavior visible rather than abstract. Flag the catch explicitly: this O(log n) promise only holds if the tree stays roughly balanced — a BST built from already-sorted input degenerates into a slow linked list, which is exactly why self-balancing variants (AVL, red-black trees) exist, even if this talk doesn't build one.

### Hash tables
A hash table maps a key to a value by running the key through a hash function that produces an array index, giving average O(1) lookup, insert, and delete — dramatically faster than scanning a list. The catch is collisions: two different keys can hash to the same index, and the structure needs a strategy (chaining a small list at that index, or probing to another slot) to handle it without losing data. Use a relatable example: a dictionary/hash map keyed by username to user profile — instead of scanning every user to find one, the hash function jumps almost directly to the right bucket. Note the tradeoff clearly: hash tables sacrifice order (there's no meaningful "first" or "next" element) to buy near-constant-time access, which is precisely why a BST is preferred when sorted order matters and a hash table is preferred when raw lookup speed is the priority.

### Synthesis and close
Return to the framing question — walk through a single running example, a contacts app, and ask which structure fits which feature: an array/list for "show all contacts in the order I added them," a hash table for "look up a contact by exact name instantly," a BST for "show me all contacts alphabetically, and let me quickly find the range 'M' through 'P'," and a stack for "undo my last edit." This makes the point concrete: real software combines several structures, each chosen for a specific access pattern, not one structure for everything. Close by explicitly naming this as a skill that pays off immediately in interviews and code reviews alike: being able to say "I chose X because we need fast Y" is a mark of a working engineer, not just a student regurgitating definitions.

## Key Takeaway
There is no universally "best" data structure — there is only the structure that matches the operations your program actually needs to be fast, so always ask "what will I do with this data most often?" before choosing.

## Go Deeper
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.006 — Introduction to Algorithms](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/pages/syllabus/)
- Related deep-dive talks in this series: `28-dijkstras-algorithm.md`, `29-graph-traversal-bfs-dfs.md`, and `30-sorting-and-searching-fundamentals.md` — natural follow-ons once these structures are familiar.
