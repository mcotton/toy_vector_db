# Detour: How Graphs Conquered Vector Search

*Optional deep-dive between Module 3C (Product Quantization) and Module 3D (HNSW)*

---

## Why this detour?

You've now built tree-based, hash-based, and compression-based indexes. Each had strengths but also clear limitations. The next module is HNSW -- the algorithm that dominates modern vector search. Before you build it, understanding *why* graphs succeeded where other approaches struggled gives you the context to appreciate what makes HNSW special.

---

## Topics

- **The history.** Navigable Small World (NSW) graphs were proposed in 2014. HNSW added hierarchical layers in 2016. Within a few years, HNSW-based systems dominated the ann-benchmarks.com leaderboard. The speed of adoption was remarkable -- it went from paper to production standard in under 5 years.

- **Why graphs work where partitioning fails.** Tree and hash methods divide the space into regions and search within them. Graphs take a fundamentally different approach: they encode *proximity relationships* directly as edges. A query navigates the graph by greedily moving toward closer neighbors. No partitions to cross, no hash buckets to miss. The graph's structure adapts to the actual data distribution rather than imposing an external structure.

- **The six degrees of separation analogy.** In a social network, any two people are connected by ~6 hops. Small-world graphs have the same property for vectors: any vector can reach any other through a small number of edges. This means greedy search converges quickly -- O(log n) hops instead of O(n) comparisons.

- **What comes after HNSW.** The field hasn't stopped. DiskANN (Microsoft, 2019) extends graph-based search to disk-resident data, achieving high recall without keeping all vectors in RAM. SPANN combines graph search with clustering for better memory efficiency. ScaNN (Google) blends quantization with graph-based re-ranking. HNSW is dominant but not the end of the story.

---

## Questions to explore

1. KD-trees partition space with hyperplanes. LSH partitions with hash functions. HNSW doesn't partition at all -- it builds a graph. Why is "not partitioning" an advantage in high dimensions?

2. In a social network, "six degrees of separation" works because there are hub nodes (celebrities, influencers) with many connections that act as shortcuts. Does HNSW have an equivalent of hub nodes? What role do the upper layers play?

3. HNSW has high memory overhead because it stores a graph (edges) on top of the vectors. Roughly, each vector might have 16-64 neighbors, each stored as an integer ID. For 100 million vectors with M=16, how much memory do just the edges require? Is this a meaningful addition to the vector storage cost?
