# Module 0: Foundations — Vectors, Distance, and Similarity

**Why this matters:** Everything in a vector DB depends on measuring how "close" two vectors are. If you don't understand this, nothing else will make sense.

## Topics
1. What is a vector in this context? (data point in high-dimensional space, not physics)
2. Embeddings: how text/images become vectors, why similar meanings produce nearby vectors
3. Dimensionality: what it means for a vector to have 384 vs 1536 dimensions
4. Distance metrics (implement all, understand trade-offs):
   - Euclidean (L2) distance
   - Cosine similarity
   - Dot product
   - Manhattan (L1) distance
   - When each is appropriate and why
5. The curse of dimensionality — why everything gets hard in high dimensions

## Deliverable
Implement all four distance functions from scratch (pure Python loops). Then rewrite with NumPy. Benchmark both. Explain the performance gap.

## Checkpoint Questions
- Compute cosine similarity of `[1, 2, 3]` and `[4, 5, 6]` by hand
- When do cosine similarity and dot product give the same nearest-neighbor ranking?
- Why does Euclidean distance become less meaningful as dimensions increase?
