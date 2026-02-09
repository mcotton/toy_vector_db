# Detour: How Production Vector DBs Design Their APIs

*Optional deep-dive between Module 4 (Filtering and Metadata) and Module 5 (Interface Design)*

---

## Why this detour?

Before you design your own API, it's worth studying what the production systems settled on. Their APIs encode years of learning about what operations users actually need, what abstractions work, and where early design decisions create long-term pain.

---

## Topics

- **Common patterns across systems.** Despite different architectures, most vector DBs converge on similar operations: upsert (insert or update), query (vector search with optional filters), delete, and collection/index management. The convergence isn't accidental -- it reflects the actual workflow of embedding pipelines.

- **Schema design choices.** Some systems (Pinecone) are schemaless -- metadata is a flat key-value bag. Others (Weaviate) enforce a schema with typed properties. Qdrant uses a middle ground with payload indexes you opt into. Each choice has consequences for query flexibility, validation, and index efficiency.

- **Upsert vs insert+update.** Most vector DBs use "upsert" (insert if new, update if exists) as the primary write operation. Why? Because embedding pipelines often re-process documents, and the caller shouldn't have to track whether an ID already exists. This is a lesson learned from real usage patterns.

- **Batch operations and streaming.** Single-vector inserts are too slow for bulk ingestion. Every production system provides batch insert (100-1000 vectors at a time). Some support streaming ingestion for real-time updates. The batch size sweet spot balances memory usage, network overhead, and throughput.

---

## Questions to explore

1. Pinecone's API uses a flat namespace with optional "namespace" partitioning. Weaviate uses "classes" with typed schemas. Chroma uses "collections." Why do you think the field hasn't converged on a single abstraction? What are the trade-offs?

2. Should your delete operation take a vector ID, a metadata filter, or both? What happens if a user deletes by filter and accidentally matches more documents than intended?

3. When a user upserts a vector with a new embedding (same ID, different vector), what has to happen to the index? Is this trivial for brute force? What about for HNSW?
