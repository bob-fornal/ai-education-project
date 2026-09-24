# Chat History — AI Entry-Level Curriculum Project

A working record of how this repository came together, kept for future reference — not a full transcript, just the requests, decisions, and reasoning worth remembering next time this project gets touched.

## Session Summary

The repo turns real university CS curricula into checklists of standalone 50-minute talks, then distills a cross-school "backbone" curriculum with full 50-minute outlines and an assessment of which subjects matter most once AI is doing a growing share of the actual coding. Built across one continuous session, in this order: seven school checklists → repo structure + backbone index → 32 talk outlines → README consolidation + AI-relevance prioritization + this history doc.

## Timeline

### 1. Seven school curriculum checklists (`curriculum/`)

Asked, one school at a time, to "do the same" for each: Amherst, Bowling Green State University (BGSU), Purdue, MIT (OpenCourseWare), Harvard (SEAS), Stanford, and Carnegie Mellon (SCS). Each became a markdown checklist of `- [ ] Talk Title` items, one talk per topic actually stated in that school's own source — never invented.

Source type varied a lot by school, and that mattered:
- **Amherst, Harvard, CMU** — the course-listing page itself had full catalog descriptions, so talks were built directly from that page.
- **BGSU, Purdue** — the listing page just linked out to per-course syllabus pages, so each course got its own fetch (batched across parallel agents for scale).
- **MIT** — OCW's search returns a messy aggregator (154 raw hits, heavy duplication, lots of pure-EE/hardware courses). Curated down to 69 real CS courses, one representative semester per course number.
- **Stanford** — the GitHub aggregator list (`isLinXu/Stanford-CS-Course`) had *no descriptions at all*, just links. Cross-referenced against Stanford's own 2002-03 Registrar bulletin PDF (which the user supplied) instead — only ~50 of ~150 current listings had a clear content match against that 23-year-old source, so the rest were listed but explicitly not fabricated.
- **CMU** — by far the largest: 574 courses across 9 SCS departments, all with real descriptions on one enormous page (~600K characters). Processed via 13 parallel batches reading line-ranges of the extracted text.

Data-quality issues surfaced and flagged rather than silently worked around: a stale/mislabeled Purdue syllabus page (CS 54100 returned 1987-dated networking content instead of a database syllabus), a BGSU course-title mismatch (catalog vs. live syllabus page disagreed), Stanford's inherent 23-year gap.

**Final tally:** ~791 courses, ~3,307 talks across the seven schools.

### 2. Repository structure + backbone curriculum index

Reorganized the seven checklist files into `curriculum/` and added a root `README.md` defining a **27-course backbone curriculum** — the canonical CS subjects that recur across most/all seven schools (Intro Programming through Computing Ethics). Each backbone row cross-references which school files cover it and links a free companion course where one exists:
- **MIT OpenCourseWare** links reused the exact syllabus URLs already curated in step 1 — no re-fetching needed.
- **Harvard PLL** (`pll.harvard.edu/catalog/free`) is a *different* catalog than the SEAS academic listing used earlier — Harvard's separate free/open course catalog (mostly CS50 spinoffs). Paginated through all 127 free listings; only 8 matched a backbone subject.

Also audited all seven school files against eleven commonly-cited "top algorithms in computer science" (the user's own list: Dijkstra's, BFS/DFS, A\*, Binary Search, Quick/Merge Sort, RSA, SHA-256, Huffman Coding, FFT, K-Means, PageRank). Six were already named somewhere; five were missing entirely and got added as new backbone checklist items.

### 3. `talks/` folder — one 50-minute outline per subject, plus a second algorithm pass

`curriculum/` answers "what could I talk about?" with many titles per school. `talks/` answers a narrower question for each of the 27 backbone subjects: what does *the single 50-minute talk* on it actually cover? Each file got a real outline (one-sentence pitch, a time-boxed segment table summing to exactly 50 minutes, real per-segment content, key takeaway) — written directly via parallel agents against a shared template, since this was original pedagogical synthesis rather than fetching.

This forced a second, independent look at the algorithm audit from step 2: *presence in the raw data* and *fits in 50 minutes* turned out to be different questions. A single "Algorithms" talk cannot do Dijkstra's, BFS, DFS, binary search, quicksort, mergesort, *and* PageRank justice — so five got pulled into dedicated talks (`28`–`32`) regardless of whether a school's checklist had already mentioned them (BFS/DFS, binary search, and the FFT had; Dijkstra's and PageRank hadn't). The other six (A\*, RSA, SHA-256, Huffman, K-Means, and one more binary-search-adjacent item) had genuine room to get full depth *inside* their natural backbone talk instead — folded in as real sections, not passing mentions.

**Final tally:** 32 talk files (27 backbone + 5 algorithm deep-dives).

### 4. README consolidation, AI-relevance prioritization, and this history doc

Three follow-up requests, handled together:
- **Reordered `README.md`** so `## What's in curriculum/` (the most granular, least-often-needed section) moved to the very bottom, behind the backbone table, the algorithm audit, and the AI-prioritization section.
- **Classified all 32 talks** (not just the 27 backbone — the user explicitly asked to include the 5 algorithm talks too) into **Critical / Important / Optional** for developers working alongside AI systems, against one explicit test: *if a developer is weak here, does AI make that weakness dangerous, or just less deep?* Landed at 13 Critical, 12 Important, 7 Optional.
- **Merged the backbone table and the talks/ index table** into one table instead of two redundant ones, after feedback that the backbone and algorithm data felt like parallel, disconnected tracks. The five algorithm deep-dives now sit inline, directly under the backbone subject they extend (e.g., Dijkstra's/BFS-DFS/Sorting/PageRank appear right after row 3, "Algorithms"; the FFT appears right after row 26, "Information Theory"), with a `↳ deep-dive` marker. The old "Canonical Algorithms Audit" section shrank to a short methodology paragraph since the placement itself now lives in the table, not in a separately repeated list.
- **This file** — moved from the repo root into `documentation/` and rewritten from a raw note-to-self (the original algorithm list, now fully worked into the repo) into an actual project history.

## Key Decisions Worth Remembering

- **Never invent talks.** Every checklist item traces to something a real source actually stated. Where a source had no real content (independent study courses, a stale Purdue page, Stanford's pre-2015 bulletin not covering post-2015 courses), that gap is flagged explicitly rather than papered over.
- **"Backbone" is curated, not computed.** No formula produced 27 — it's the set of subjects that were both common across at least three schools and distinct enough to deserve their own line rather than folding into a neighbor.
- **Algorithm placement uses two independent tests**, not one: does the raw curriculum data already name it, and separately, does it fit in a shared 50 minutes or need its own talk. They don't always agree, and both outcomes are marked directly in the backbone table.
- **The AI-relevance framework is a judgment call, stated explicitly** so it can be re-argued later: weak-plus-AI-equals-dangerous is "Critical"; weak-plus-AI-equals-just-less-deep is "Optional."

## Original Working Note (now fully incorporated)

The line below is preserved as a record of the original instruction that seeded the algorithm audit in step 2/3 above — kept for provenance, not as a live to-do (everything in it is now reflected in `curriculum/`, the root `README.md`, and `talks/28`–`32`, `talks/16`, `talks/17`, `talks/18`, and `talks/26`):

> The most widely used algorithms in computer science span across sorting, searching, cryptography, and graph theory — Dijkstra's Algorithm, BFS/DFS, A\* Search, Binary Search, Quick Sort & Merge Sort, RSA, SHA-256, Huffman Coding, the Fast Fourier Transform, K-Means Clustering, and PageRank.
