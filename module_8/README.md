# Module 8: Advanced Topics (Optional)

**Why this exists:** These are real-world concerns that production vector databases deal with. Each is a rabbit hole worth exploring if you have the time and interest.

## Before You Start

No readiness gate here — this module is a menu, not a sequence. Pick topics that interest you. Each is largely independent, though some build on concepts from earlier modules.

By this point, you've built a working vector database with search, persistence, multiple index types, filtering, and real embeddings. That covers the core of what a vector database does. But production systems face additional challenges that our toy database sidesteps: what happens when data changes? When multiple users share the system? When the data outgrows a single machine? Each topic below addresses one of these real-world concerns.

Skim the list and pick 1-2 that you're most curious about. For each, think about: why would a production system need this? What problem does it solve that our toy database doesn't handle?

## Topics

Choose any that interest you:

- **Dimensionality reduction (PCA, random projection):** Can you reduce 768 dims to 128 and still get good recall? When is this worth the precision loss? Connects to Module 3C (PQ also compresses, but differently).

- **Hybrid search (BM25 + vector):** Keyword search and semantic search find different things. Some queries need exact keyword matches ("error code 0x4F2A"), others need meaning ("how to fix authentication failures"). How do you combine both? This is what production search engines actually do.

- **Updates and deletes in ANN indexes:** You've seen that HNSW degrades with deletes (Module 3D). How do production systems handle mutable data? Tombstones, rebuild strategies, copy-on-write — each has trade-offs.

- **Multi-tenancy and access control:** If 100 different customers share one vector database, how do you ensure customer A can't search customer B's data? Filtered search (Module 4) is part of the answer, but not all of it.

- **Distributed/sharded vector search:** Your database runs on one machine. What happens when the data exceeds one machine's capacity? How do you split the index across machines? How do you merge results from multiple shards?
