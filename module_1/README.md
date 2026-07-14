# Module 1: Brute-Force Search (The Baseline)

**Why this matters:** You need a correct, simple baseline before you can evaluate any optimization. Every index is trying to beat this.

## Before You Start

Answer these readiness questions:
1. Can you explain what Euclidean distance and cosine similarity measure, and when you'd use each?
2. If you have 1000 vectors and a query, how would you find the most similar one? (Plain English is fine.)
3. Have you used a Python class before? Are you comfortable with `__init__`, methods, and `self`?
4. Do you know what Big-O notation means? (Even a rough sense like "O(n) means it scales linearly" is enough.)

**If you're unsure on #1 or #2**, go back to Module 0. The rest of this module won't make sense without solid distance metric intuition. **If #3 or #4 are shaky**, that's OK — we'll work through them, but expect to move slower.

## Introduction

In Module 0 you learned to measure the distance between two vectors. Now you'll search an entire collection: given a query vector, find the k closest stored vectors and return them in order.

The simplest approach is **brute force**: compare the query against every stored vector, keep track of the best results, return the top k. Imagine looking for a book in an unsorted pile — you have no choice but to check every single one. That's brute force. It's slow, but it's always correct, and that makes it the gold standard for evaluating every optimization you'll build later.

**A note on time complexity (Big-O):** When we say brute-force search is O(n), we mean the time it takes grows linearly with the number of vectors. Double the vectors, double the search time. O(n * d) means it also grows linearly with dimensionality — each distance calculation involves d numbers. You don't need to be a Big-O expert for this module, but you should understand the basic idea: it's a shorthand for "how does the work scale as the input gets bigger?"

This module also introduces a data structure decision: how do you efficiently track the "top k" results as you scan? One approach is to compute all distances, sort them, and take the first k. That works but does unnecessary work — you don't need a fully sorted list, just the k smallest values. A **heap** is a data structure that maintains a running "top k" efficiently. Think of it like a bouncer at a club with a capacity of k: every time a new candidate shows up, you only let them in if they're better than the worst person currently inside — and when they come in, the worst person gets kicked out, so the club stays at exactly k. The heap makes this check fast. You'll understand why and implement it.

By the end of this module you'll have a working `VectorStore` class that you'll keep improving throughout the course.

## Topics
1. Linear scan: compare query against every stored vector
2. k-nearest neighbors (kNN) — returning top-k, not just top-1
3. Using a heap for efficient top-k selection
4. Vectorized selection: `np.argpartition` for O(n) top-k without sorting
5. Time complexity: O(n * d) per query
6. Space complexity: O(n * d) for storage

## Optional: Visualizing Search Time Scaling

**Goal:** See how brute-force search time grows with dataset size, and understand what O(n * d) actually feels like.

**Exercise:** Time your `search()` method across different dataset sizes (100, 1000, 10000, 100000 vectors) at a fixed dimensionality (e.g., 128). Plot dataset size vs. query time. Is the relationship linear? Then fix n=10000 and vary dimensionality (8, 64, 256, 768). Plot dimensions vs. query time. Is that linear too?

**Key insight:** Brute force is predictable — time scales linearly with both n and d. There are no surprises, no worst cases worse than average. That predictability is actually a feature: every index you build later will have variable performance, and you'll compare against this straight line.

If you'd rather skip the visualization, ask your instructor to generate the timing script and plot for you.

## Deliverable
Build a `VectorStore` class:
- `insert(id: str, vector: list[float], metadata: dict)`
- `search(query_vector: list[float], k: int) -> list[tuple[str, float]]`
- Returns IDs and distances of k nearest neighbors

Implement top-k selection **three ways**, and benchmark all three:
1. A max-heap (`heapq`) maintained during the scan — feel the O(n log k)
2. Compute all distances, sort, slice — the naive O(n log n)
3. Vectorized distances + `np.argpartition` — O(n) selection, no Python loop

Explain each performance gap. The heap loses to argpartition in Python — understanding *why* (per-element interpreter overhead vs. vectorized C) is the point, and it previews the memory-layout lesson in Module 2.

**Grading:** your instructor writes and runs a test suite against this deliverable — it must pass, along with Module 0's suite (see `OVERVIEW.md` → Verification and Grading).

## Checkpoint Questions
- 1M vectors, 768 dimensions: how many float operations per query?
- At what scale does brute force become unacceptable? What's "unacceptable"?
- Why use a max-heap instead of sorting all results? Why does `np.argpartition` beat both in Python anyway?
