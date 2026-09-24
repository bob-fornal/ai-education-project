# Database Systems

**Backbone Course #11** · **Duration:** 50 minutes

## The One-Sentence Pitch
Databases let you describe *what* data you want, not *how* to fetch it, while quietly guaranteeing that your data stays organized, fast to search, and safe even when things go wrong mid-operation.

## Audience & Prerequisites
This talk is for learners who understand basic data structures like lists and can write simple programs; no prior SQL or database experience is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: why not just use a giant spreadsheet |
| 0:05–0:15 | The relational model and SQL |
| 0:15–0:26 | Normalization |
| 0:26–0:36 | Indexing |
| 0:36–0:46 | Transactions and ACID |
| 0:46–0:50 | Key takeaway and close |

### Hook: why not just use a giant spreadsheet
Open by asking: why don't we just store all of an application's data in one giant spreadsheet? Walk through what breaks — a customer's address gets duplicated across every order they've ever placed, so fixing a typo means editing hundreds of rows; two people editing at once can silently overwrite each other's changes; and finding "all orders over $100 from last week" means scanning every single row by hand. Databases exist to solve exactly these problems at scale: organizing data to avoid duplication, letting you ask precise questions without scanning everything, and protecting against corruption when multiple things happen at once. Every topic in this talk is a specific answer to one of those three problems.

### The relational model and SQL
The relational model organizes data into tables, where each row is one record and each column is one attribute — a `students` table might have columns for id, name, and major, with one row per student. The powerful shift SQL brings is declarative querying: instead of writing a loop that manually scans rows and checks conditions (the "how"), you write a statement like `SELECT name FROM students WHERE major = 'CS'` that describes only the result you want (the "what"), and the database's internal query engine figures out the most efficient way to actually retrieve it. This separation matters enormously in practice — the database can change its retrieval strategy (use an index, reorder operations, parallelize) without you ever having to rewrite your query, because you never told it *how* in the first place.

### Normalization
Normalization is the discipline of splitting data into multiple related tables instead of one giant flat table, specifically to eliminate duplication and the bugs that come with it. The formal justification involves functional dependencies — the idea that one column's value determines another's (a `zip_code` determines a `city`, for instance) — and normalization rules essentially say "don't store a fact in more places than the one table whose key actually determines it." Concretely: instead of repeating a customer's address on every one of their orders, you store customers in one table and orders in another, linking them by a customer ID — now the address lives in exactly one place, and updating it once updates it everywhere it's used. The tradeoff to mention honestly: highly normalized data sometimes requires combining multiple tables back together at query time (a "join"), which is the price paid for not duplicating information.

### Indexing
Without help, finding a specific row means scanning the entire table from top to bottom — fine for a hundred rows, unusable for a hundred million. An index is an auxiliary structure, commonly a B-tree, that lets the database jump almost directly to the relevant rows instead of scanning everything, much like a book's index lets you jump to a page instead of reading cover to cover. The B-tree intuition: it's a balanced, sorted tree structure where each level narrows the search dramatically, so finding one row among a million takes roughly 20 comparisons instead of a million. The tradeoff to flag: an index speeds up reads on the column it covers but adds overhead to every write (since the index must be updated too), which is why databases don't index every column by default — you choose indexes based on what your application actually searches by.

### Transactions and ACID
A transaction is a group of operations that the database treats as a single, indivisible unit — either all of them happen, or none of them do. The canonical example is a bank transfer: subtracting $100 from account A and adding $100 to account B are two separate operations, but if the system crashes after the subtraction and before the addition, $100 has simply vanished — wrapping both in a transaction guarantees that either both happen or neither does, so the money is never lost mid-operation. This guarantee is formalized as ACID: Atomicity (all-or-nothing), Consistency (the database never ends up in a state that violates its own rules), Isolation (concurrent transactions don't see each other's half-finished work), and Durability (once committed, a transaction survives even a crash immediately after). Each letter answers a specific failure mode — Atomicity answers "what if we crash halfway," Isolation answers "what if two things happen at the same time" — and together they're why databases, not plain files, are trusted with anything financially or operationally critical.

### Key takeaway and close
Tie the four topics back to the opening spreadsheet problem: the relational model plus SQL solves "ask precise questions efficiently," normalization solves "don't duplicate and desynchronize data," indexing solves "don't scan everything every time," and ACID transactions solve "don't corrupt data when things go wrong or happen concurrently." None of these are academic exercises — they are direct, practical responses to real failures that plain file or spreadsheet storage cannot prevent.

## Key Takeaway
A database is not just organized storage — it's a set of guarantees (declarative querying, non-duplicated data, fast lookup, all-or-nothing transactions) that plain files can't offer, and understanding those guarantees is what separates using a database from merely storing data in one.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.814 — Database Systems](https://ocw.mit.edu/courses/6-830-database-systems-fall-2010/pages/syllabus/)
- Harvard PLL: [CS50's Introduction to Databases with SQL](https://pll.harvard.edu/course/cs50s-introduction-databases-sql)
