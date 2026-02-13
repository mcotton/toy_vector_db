# Module 3C: Indexing — Product Quantization (PQ)

**Why third:** Shifts focus from "search faster" to "use less memory." Critical concept for scale.

## Before You Start

Answer these readiness questions:
1. How much memory does 1 million vectors at 768 dimensions (float32) require? Compute it.
2. What about 100 million vectors? 1 billion? Could those fit in RAM on a typical server?
3. Do you know what k-means clustering does? (Even a rough idea like "groups similar points together" is enough.)
4. What's the difference between lossless and lossy compression? Can you give a real-world example of each?

**If #1/#2 are unclear**, do the arithmetic: `n_vectors * dimensions * 4 bytes`. This module is motivated by that math — at scale, raw vectors don't fit in memory. **If #3 is totally new**, spend 15 minutes reading about k-means before starting — PQ depends on it heavily. You don't need to implement it (we'll use scikit-learn or NumPy), but you need the intuition of "find k cluster centers that represent the data." **#4** frames the core trade-off: PQ is lossy compression for vectors.

## Introduction

KD-Trees tried to search faster by skipping vectors. LSH tried to search faster by hashing similar vectors together. Product Quantization attacks a different problem: **what if you can't even fit all your vectors in memory?**

1 million 768-dim float32 vectors = ~3 GB. Manageable. But 100 million = ~300 GB, and 1 billion = ~3 TB. No single machine has that much RAM. You need compression.

PQ compresses each vector from `d * 4 bytes` down to `m bytes` (where m is typically 8-96), achieving 30-200x compression. The trick: instead of compressing the whole vector at once, PQ splits it into **subvectors** and compresses each independently.

Here's how it works:
1. **Split** each d-dimensional vector into m subvectors (e.g., 768 dims → 96 subvectors of 8 dims each)
2. **Train** a small k-means codebook (typically 256 centroids) on each subspace independently
3. **Encode** each subvector as the ID of its nearest centroid (1 byte if 256 centroids)
4. **Search** by precomputing distances from the query's subvectors to all centroids, then look up and sum

The result: instead of storing 768 floats (3072 bytes), you store 96 centroid IDs (96 bytes). 32x compression. The cost is precision — you're measuring distance to centroids, not to the original vectors.

## Topics
- Subvector decomposition: splitting a d-dimensional vector into m subvectors
- Learning codebooks via k-means on each subspace
- Encoding vectors as sequences of centroid IDs
- Asymmetric distance computation (ADC)
- Compression ratio: from `d * 4 bytes` to `m * 1 byte`

## Deliverable
Implement PQ encoding and asymmetric distance search. Measure memory savings and recall degradation vs. exact search.

## Checkpoint Questions
- If d=768 and m=96, how many subvectors? What dimension is each?
- Why asymmetric distance (query is not quantized) vs. symmetric (both quantized)?
- What's the trade-off of more subvectors (larger m)?
