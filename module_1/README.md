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

The simplest approach is **brute force**: compare the query against every stored vector, keep track of the best results, return the top k. It's slow — O(n) comparisons per query — but it's always correct, and that makes it the gold standard for evaluating every optimization you'll build later.

This module also introduces a core data structure decision: how do you efficiently track the "top k" results as you scan? Sorting all results works but is wasteful. A **heap** gives you the top k in O(n log k) — you'll understand why and implement it.

By the end of this module you'll have a working `VectorStore` class that you'll keep improving throughout the course.

## Topics
1. Linear scan: compare query against every stored vector
2. k-nearest neighbors (kNN) — returning top-k, not just top-1
3. Using a heap for efficient top-k selection
4. Time complexity: O(n * d) per query
5. Space complexity: O(n * d) for storage

## Deliverable
Build a `VectorStore` class:
- `insert(id: str, vector: list[float], metadata: dict)`
- `search(query_vector: list[float], k: int) -> list[tuple[str, float]]`
- Returns IDs and distances of k nearest neighbors

## Checkpoint Questions
- 1M vectors, 768 dimensions: how many float operations per query?
- At what scale does brute force become unacceptable? What's "unacceptable"?
- Why use a max-heap instead of sorting all results?
