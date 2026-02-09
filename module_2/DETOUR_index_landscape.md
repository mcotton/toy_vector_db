# Detour: The Indexing Landscape -- What Production Systems Actually Use

*Optional deep-dive between Module 2 (Storage and Persistence) and Module 3A (KD-Trees)*

---

## Why this detour?

Before you build specific index types, it's worth seeing the full landscape. Knowing what real systems chose and why gives you a map for the territory you're about to explore.

---

## Topics

- **The speed/recall/memory triangle.** Every index trades between query speed, search accuracy (recall), and memory consumption. You can usually pick two. Brute force has perfect recall but terrible speed. Heavy quantization saves memory but hurts recall. HNSW is fast and accurate but memory-hungry.

- **What real vector databases chose.** FAISS (Meta) uses IVF+PQ as its workhorse -- optimized for billion-scale on GPUs. Pinecone combines graph-based search with quantization. Weaviate defaults to HNSW. Qdrant uses HNSW with quantization options. None use brute force as their primary path.

- **Why there's no single "best" index.** A recommendation system serving 10 QPS over 1M vectors has completely different requirements than a search engine serving 10,000 QPS over 1B vectors. Index selection is driven by dataset size, query volume, latency requirements, memory budget, and acceptable recall loss.

- **The evolution of the field.** Brute force → tree-based (KD-trees, 1990s) → hash-based (LSH, 2000s) → quantization (PQ) and clustering (IVF, 2010s) → graph-based (HNSW, mid-2010s). Each generation addressed limitations of the previous. You'll build representatives of each family.

---

## Questions to explore

1. A startup has 500K document embeddings (768 dims, float32) and ~50 QPS. Their server has 16 GB RAM. Which index family would you recommend? What if they grew to 50 million documents?

2. FAISS's most popular config is IVF+PQ. Why combine two techniques? What does each buy you?

3. If HNSW consistently tops ann-benchmarks.com for recall vs speed, why doesn't every system just use HNSW? What's the catch?
