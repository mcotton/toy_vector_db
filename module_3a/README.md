# Module 3A: Indexing — KD-Trees (Tree-Based Partitioning)

**Why first:** Intuitive spatial partitioning. Easy to visualize. Understanding why it fails in high dimensions is the key lesson.

## Before You Start

Answer these readiness questions:
1. Have you ever implemented a tree data structure (binary tree, BST, trie, etc.)? If not, that's OK — we'll build a basic binary tree first before tackling KD-Trees.
2. Are you comfortable with recursion? Can you write a function that calls itself with a smaller input and has a base case?
3. How does your current brute-force search work? What's its time complexity?
4. If you had a sorted list of numbers and needed to find one quickly, what approach would you use? What's its time complexity?

**If #1 is "no"**, plan for extra time — this module starts by building a basic binary tree from scratch to establish the fundamentals (nodes, left/right children, recursive construction, traversal) before extending to KD-Trees. **If #2 is shaky**, practice with a simple recursive function (factorial, fibonacci) first. **#3 should be solid** from Module 1 — you need the baseline to compare against. **#4** connects to the core idea: trees enable search by elimination.

## Introduction

Brute force compares your query against every stored vector. Can we do better? What if we could organize vectors spatially — like sorting a list lets you binary search instead of scanning — so we only check a fraction of them?

A **KD-Tree** (k-dimensional tree) does exactly this. It recursively splits the vector space in half, alternating which dimension it splits on. When searching, you traverse the tree to find the region where your query falls, then check whether neighboring regions might contain closer points (backtracking).

In low dimensions (2D, 3D), this is powerful — you might visit only 10-20 nodes out of thousands. But as dimensions increase, something goes wrong. Discovering *what* and *why* is the real lesson of this module. KD-Trees are historically important but fail in high dimensions, and understanding that failure motivates everything that comes after (LSH, PQ, HNSW).

**Scaffolding for this module:**
1. Build a basic binary tree (if you haven't before) — Node class, recursive insert, display
2. Extend to KDNode with split dimensions — understand how 2D/3D space gets partitioned
3. Implement construction — recursive median split, alternating dimensions
4. Implement search — traversal first (visit every node), then add backtracking and pruning
5. Test at increasing dimensions — observe where it breaks down

## Topics
- Recursive binary space partitioning
- Choosing split dimensions and split values (median)
- Search: traversal + backtracking
- Why performance degrades exponentially with dimensions

## Deliverable
Implement a KD-Tree index. Run search on 3-dim, 100-dim, and 768-dim data. Compare recall and speed against brute force.

## Implementation Notes
- **Build step uses `sorted()` instead of median selection.** This is a pedagogical simplification — sorting at each recursive level gives O(n log² n) build time instead of O(n log n) with linear-time median selection (e.g., `np.partition`). Standard KD-Tree implementations use the faster approach. Keep this in mind when analyzing build-time complexity or comparing benchmarks against reference implementations.

## Checkpoint Questions
- At what dimensionality does your KD-Tree become slower than brute force? Why?
- How does the tree depth relate to the number of points?
- What's the worst-case query time?
