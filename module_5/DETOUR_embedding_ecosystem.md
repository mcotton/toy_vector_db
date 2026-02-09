# Detour: The Embedding Model Ecosystem

*Optional deep-dive between Module 5 (Interface Design) and Module 6 (Connecting to Real Embeddings)*

---

## Why this detour?

You're about to plug real embedding models into your vector DB. Before you do, you should understand the landscape of available models, how they differ, and how those differences affect your system's behavior. Picking the wrong model is a more common failure mode than picking the wrong index.

---

## Topics

- **Open vs API models.** Open models (sentence-transformers, BGE, E5, GTE) run locally -- free, private, no network dependency, but you manage the compute. API models (OpenAI, Cohere Embed, Voyage) are hosted -- easy to start, but add latency, cost per call, and a dependency on an external service. Most production systems support both.

- **Dimension choices and trade-offs.** Models produce vectors of different sizes: 384 (MiniLM), 768 (BERT-base, BGE), 1024 (E5-large), 1536 (OpenAI ada-002), 3072 (OpenAI text-embedding-3-large). Larger dimensions capture more nuance but cost more to store and search. The relationship between dimension and quality is not linear -- doubling dimensions does not double quality.

- **MTEB benchmark.** The Massive Text Embedding Benchmark ranks models across tasks: retrieval, classification, clustering, semantic similarity. It's the standard way to compare embedding models. But benchmark performance doesn't always match your specific domain -- a model that wins MTEB might underperform on legal documents or code.

- **Asymmetric vs symmetric embeddings.** Some models are trained for symmetric similarity (sentence A similar to sentence B). Others are asymmetric (short query similar to long document). Using a symmetric model for query-document retrieval can underperform because queries and documents have different characteristics. This is a subtle but important distinction.

- **When to fine-tune.** Off-the-shelf models work well for general text. For specialized domains (medical, legal, code), fine-tuning on domain data can dramatically improve recall. But fine-tuning requires labeled pairs and invalidates all previously stored embeddings -- you have to re-embed everything.

---

## Questions to explore

1. You switch from a 384-dim model to a 1536-dim model. How does this affect your vector DB's memory usage, search latency, and index build time? Is the quality improvement worth the cost?

2. Your system uses OpenAI's embedding API. One day their API goes down for 3 hours. What breaks in your pipeline? What would you need to do differently to be resilient to this?

3. You fine-tune an embedding model on your domain data and get 15% better recall. But you already have 50 million vectors embedded with the old model. What are your options? Which is least painful?
