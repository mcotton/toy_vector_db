# Module 1: Brute-Force Search (The Baseline)

**Why this matters:** You need a correct, simple baseline before you can evaluate any optimization. Every index is trying to beat this.

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
