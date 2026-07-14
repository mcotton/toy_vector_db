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

**What is lossy compression?** Think about JPEG images. A raw photo might be 24 MB; a JPEG version is 2 MB. The JPEG looks almost identical, but if you zoom in, fine details are gone. You traded precision for size, and the original data can't be recovered. PQ does the same thing for vectors — it's the JPEG of vector search.

**What is k-means?** K-means is a clustering algorithm. Given a pile of data points and a number k, it finds k "representative" points (called **centroids**) that summarize the data. The algorithm is simple: (1) place k centroids randomly, (2) assign each data point to its nearest centroid, (3) move each centroid to the average position of its assigned points, (4) repeat steps 2-3 until the centroids stop moving. The result: k centroids that capture the general shape of the data. You don't need to implement k-means yourself (scikit-learn provides it), but understanding the idea is essential for PQ.

**What is a codebook?** A codebook is just a lookup table: a list of representative values (centroids), each identified by an ID number. Instead of storing the original value, you store the ID of the closest representative. Think of it like a paint color chart — instead of storing the exact RGB color `(142, 87, 203)`, you store "color #47" and look up the RGB value when you need it. The codebook is the chart; the ID is the swatch number.

**How PQ works:**

PQ compresses each vector from `d * 4 bytes` down to `m bytes` (where m is typically 8-96), achieving 30-200x compression. The trick: instead of compressing the whole vector at once, PQ splits it into **subvectors** and compresses each independently.

1. **Split** each d-dimensional vector into m subvectors (e.g., 768 dims → 96 subvectors of 8 dims each)
2. **Train** a small k-means codebook (typically 256 centroids) on each subspace independently
3. **Encode** each subvector as the ID of its nearest centroid (1 byte if 256 centroids — values 0-255 fit in a single byte)
4. **Search** by precomputing distances from the query's subvectors to all centroids, then look up and sum

The result: instead of storing 768 floats (3072 bytes), you store 96 centroid IDs (96 bytes). 32x compression. The cost is precision — you're measuring distance to centroids, not to the original vectors. Like JPEG, you're betting that "close enough" is good enough.

## Topics
- Subvector decomposition: splitting a d-dimensional vector into m subvectors
- Learning codebooks via k-means on each subspace
- Encoding vectors as sequences of centroid IDs
- Asymmetric distance computation (ADC)
- Compression ratio: from `d * 4 bytes` to `m * 1 byte`

## Optional: Visualizing Quantization Error

**Goal:** See what lossy compression actually looks like — how much information is lost when you replace a subvector with its nearest centroid.

**Exercise:** Take a small set of 2D points (these represent one subvector space). Run k-means with k=4 centroids. Plot the original points, the centroids, and draw lines from each point to its assigned centroid. The length of each line is the quantization error for that point.

Then try k=8 and k=16 centroids. How does the average error change? What happens to points that are far from any centroid?

**Extension:** Generate 1000 random 768-dim vectors. Split into m=96 subvectors (8 dims each). Run k-means with 256 centroids on one subspace. Encode and decode the subvectors (replace each with its centroid). Compare the original distance between two vectors vs. the reconstructed distance. How much error does quantization introduce?

**Key insight:** PQ error is not uniform — vectors near cluster boundaries get distorted more than vectors near centroids. This means recall degradation depends on the data distribution, not just the compression ratio.

If you'd rather skip the visualization, ask your instructor to generate the plots for you.

## Deliverable
Implement PQ encoding and asymmetric distance search. Measure memory savings and recall degradation vs. exact search.

**Grading:** your instructor writes and runs a test suite against this deliverable — it must pass, along with all previous modules' suites (see `OVERVIEW.md` → Verification and Grading).

## Where PQ Goes From Here

On its own, PQ is compression, not a search structure — searching PQ codes still means scanning every stored vector. Production systems pair it with a coarse partitioning index so you only scan a fraction of the codes. That pairing is IVF-PQ, the workhorse of billion-scale search (it's what FAISS is famous for). **Module 3E (optional)** builds IVF and then combines it with the PQ you just wrote — best taken now, while k-means is fresh in your head.

## Checkpoint Questions
- If d=768 and m=96, how many subvectors? What dimension is each?
- Why asymmetric distance (query is not quantized) vs. symmetric (both quantized)?
- What's the trade-off of more subvectors (larger m)?
