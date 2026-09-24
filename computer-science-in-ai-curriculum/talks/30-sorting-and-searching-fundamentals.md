# Sorting & Searching Fundamentals: Binary Search, Quick Sort & Merge Sort

*This talk was split out from the general "Algorithms" backbone talk (see `03-algorithms.md`) — sorting and searching are the two algorithm families nearly every programmer uses directly (often via a standard library call), so they're grouped into one dedicated session rather than compressed into the broader survey.*

## The One-Sentence Pitch
Binary search, quick sort, and merge sort are the three algorithms that quietly power nearly every "find" and "sort" operation in software, and understanding how each one works reveals real, practical tradeoffs, not just theory.

## Audience & Prerequisites
For learners comfortable with basic recursion and Big-O notation at an intuitive level; no prior sorting/searching-algorithm background required.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: you already call these algorithms daily |
| 0:05–0:15 | Binary Search |
| 0:15–0:28 | Quick Sort |
| 0:28–0:40 | Merge Sort |
| 0:40–0:47 | Comparing the three, and what real libraries do |
| 0:47–0:50 | Key takeaway and close |

### Hook: you already call these algorithms daily
Open by pointing out that every programmer has called `sorted()`, `Arrays.sort()`, or an equivalent, and every programmer has looked something up in a sorted list or dictionary — but few have looked inside those calls. This talk opens that hood: three algorithms, all decades old, that remain the backbone of how modern software sorts and searches data, and understanding their mechanics explains real performance differences you'll actually run into.

### Binary Search
Introduce binary search as the simplest and most instructive of the three. The setup: given a sorted array and a target value, repeatedly check the middle element — if it matches, done; if the target is smaller, discard the entire right half and repeat on the left half; if larger, discard the left half and repeat on the right. Emphasize the precondition explicitly and why it matters: this only works on already-sorted data — binary search on an unsorted array gives meaningless results, because discarding a half relies on knowing everything in it is either all-too-small or all-too-large. Walk the complexity intuition: each step eliminates half the remaining candidates, so the number of steps needed is the number of times you can halve n before reaching 1, which is log₂(n) — for a million-element array, that's about 20 comparisons instead of up to a million for a linear scan. Land the payoff concretely: on a sorted array of a billion elements, binary search takes about 30 steps; a linear scan could take up to a billion — this is why sorting data up front, even at some cost, is often worth it if you'll be searching it many times afterward.

### Quick Sort
Walk through quick sort's mechanism: pick a pivot element from the array, partition the rest of the array so everything smaller than the pivot ends up on one side and everything larger ends up on the other (the pivot lands in its final sorted position in this step), then recursively apply the same process to the left and right partitions independently. Explain the complexity story honestly: on average, a well-chosen pivot splits the array roughly in half each time, giving O(n log n) performance, matching the best sorting algorithms can do. But the worst case is O(n²) — if the pivot chosen is consistently the smallest or largest element (which happens, for example, with naive "always pick the first element" pivoting on already-sorted or adversarial data), partitions become wildly lopsided and the recursion barely shrinks the problem each time. Explain why practical implementations avoid this: choosing the pivot randomly, or using "median-of-three" (comparing the first, middle, and last elements and picking the median of those as the pivot), makes the worst case extremely unlikely to trigger in practice, which is why real-world quick sort implementations are fast almost all the time despite the theoretical worst case existing.

### Merge Sort
Walk through merge sort's mechanism: recursively split the array in half, then split those halves in half, continuing all the way down until every sub-array is a single element (trivially "sorted"). Then merge back up: combine two sorted sub-arrays into one sorted array by repeatedly comparing their front elements and taking the smaller one, continuing until both are exhausted — then merge those results together one level up, and so on until the whole array is reassembled, fully sorted. Contrast its guarantees against quick sort's: merge sort's worst case is always O(n log n), no exceptions, no unlucky pivot scenario — the split-in-half-every-time structure is completely independent of the data's arrangement. The tradeoff: merge sort needs extra space proportional to the array size to hold the temporary merged sub-arrays, whereas quick sort's partitioning can be done in place within the original array, using much less extra memory. This makes merge sort attractive when a worst-case guarantee matters more than memory usage — for example, sorting linked lists (where merge sort's sequential merging fits naturally) or external sorting of data too large to fit in memory (merging sorted chunks from disk).

### Comparing the three, and what real libraries do
Bring the three together with a comparison in words. Binary search: O(log n) search, but only works on already-sorted data — the sorting cost has to be paid somewhere first. Quick sort: average O(n log n), worst case O(n²), in-place (low memory overhead), not naturally stable (equal elements can be reordered relative to each other). Merge sort: guaranteed O(n log n) in all cases, needs extra memory, and is naturally stable (equal elements keep their original relative order) — which matters when sorting records by one field while wanting ties to preserve a previous order. Close by noting that production standard libraries rarely use a pure textbook version of either: Java's `Arrays.sort()` uses a tuned variant of merge sort (or Timsort-like hybrids) for objects and dual-pivot quicksort for primitives, while Python's `sorted()` uses Timsort, a hybrid that exploits merge sort's stability and worst-case guarantees while borrowing tricks to run fast on real-world, partially-ordered data. The lesson: understanding the classic algorithms explains why these hybrids make the engineering choices they do.

### Key takeaway and close
Recap the three tools and when each shines: binary search for fast lookups once data is sorted, quick sort for fast average-case in-place sorting, and merge sort for a guaranteed worst case and stability — and remember that the sort function you call every day is a carefully engineered hybrid standing on exactly these ideas.

## Key Takeaway
Binary search, quick sort, and merge sort each make a different tradeoff — search speed versus a sortedness precondition, average speed versus memory, and worst-case guarantees versus extra space — and the standard library sort you call without thinking is a hybrid engineered around exactly those tradeoffs.

## Go Deeper
- Backbone home: `03-algorithms.md` in this folder
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md), [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md), and [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) all cover sorting and searching algorithms directly
- Source: [GeeksforGeeks — Binary Search](https://www.geeksforgeeks.org/dsa/binary-search/)
