# Detour: Real Queries Are Never Just "Find Similar"

*Optional deep-dive between Module 3D (HNSW) and Module 4 (Filtering and Metadata)*

---

## Why this detour?

You've built four different indexing strategies. They all solve the same problem: given a query vector, find the nearest vectors. But in production, users almost never want *just* that. They want "find me similar products **under $50 in the electronics category**" or "find relevant documents **that this user has access to**." This gap between pure vector search and real-world queries is where most of the engineering complexity lives.

---

## Topics

- **Metadata filtering is the #1 feature request.** Every production vector database supports filtering. Pinecone, Weaviate, Qdrant, and Milvus all treat it as a core feature, not an add-on. The reason: vector similarity alone isn't enough for real applications. Multi-tenancy (each user sees only their data), access control, categorical constraints, and range filters are table stakes.

- **Hybrid search: keywords + vectors.** Pure vector search misses exact keyword matches. Pure keyword search misses semantic similarity. Production systems increasingly combine both: BM25 (keyword relevance) + vector similarity, merged with a fusion algorithm like Reciprocal Rank Fusion (RRF). This is why Elasticsearch added vector search and why vector DBs are adding keyword indexes.

- **Re-ranking pipelines.** Vector search is often just the first stage. A common pattern: retrieve 100-1000 candidates with fast ANN search, then re-rank them with a more expensive cross-encoder model that looks at the query and each candidate together. The vector DB provides recall; the re-ranker provides precision.

- **The gap between benchmarks and reality.** ann-benchmarks.com measures pure vector search on uniform random data with no filters. Real workloads have skewed distributions, metadata filters that eliminate 90% of candidates, and concurrent writes during search. A system that dominates benchmarks might struggle in production if it doesn't handle these realities.

---

## Questions to explore

1. Your vector index returns the 10 nearest vectors, but 8 of them fail the metadata filter. You only have 2 valid results instead of 10. What are three different strategies to handle this, and what are the trade-offs of each?

2. In a multi-tenant system (1000 users, each with their own documents), you could build one shared index with a "user_id" filter, or 1000 separate indexes. What are the trade-offs? When would each approach win?

3. A cross-encoder re-ranker is much more accurate than vector similarity but 100x slower. If you retrieve top-100 candidates with ANN and re-rank with the cross-encoder, what's the overall latency profile? At what retrieval depth does the re-ranker become the bottleneck?
