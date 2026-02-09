# Detour: How Embedding Models Are Trained

*Optional deep-dive between Module 0 (Distance Functions) and Module 1 (Brute-Force Search)*

---

## Why this detour?

You just implemented distance functions that compare vectors. But where do those vectors come from? They come from embedding models -- neural networks trained specifically to place semantically similar items close together in vector space. Understanding how these models are trained changes the way you think about the vectors you're searching over. It explains why some vectors cluster well and others don't, why distance metric choice matters, and why "garbage in, garbage out" starts at the embedding layer, not the database layer.

---

## Topics

- **Contrastive learning as the core idea.** Embedding models learn by seeing pairs (or triples) of examples and being told "these two are similar" and "these two are different." The model adjusts its weights so similar items end up with small distances and dissimilar items end up with large distances. This is fundamentally different from classification -- the model doesn't learn categories, it learns a geometry.

- **Anchor, positive, negative -- the triplet framework.** The classic training setup: take an anchor example (a sentence, an image), a positive (something semantically similar), and a negative (something dissimilar). The loss function pushes the anchor closer to the positive and farther from the negative. Triplet loss was the original approach; InfoNCE (used in CLIP and modern sentence transformers) generalizes this by contrasting against many negatives at once.

- **Hard negatives make or break training quality.** A "hard negative" is an example that looks superficially similar but is semantically different -- like "How to fix a flat tire" vs "How to fix a flat rate mortgage." Random negatives are too easy; the model learns nothing from them. Finding good hard negatives is one of the most important (and difficult) parts of training an embedding model.

- **In-batch negatives -- a clever efficiency trick.** Instead of explicitly mining negatives, you treat every other example in the training batch as a negative for each anchor. A batch of 1024 pairs gives you 1023 negatives for free. This is why large batch sizes matter so much for contrastive learning -- more negatives means a harder, more informative training signal.

- **Why training quality flows downstream to your vector DB.** If the embedding model places unrelated items close together, no amount of indexing cleverness will fix your search results. The quality ceiling of a vector database is set by the embedding model. This is why the field obsesses over benchmarks like MTEB -- they measure how well the geometry of the embedding space matches human notions of similarity.

---

## Questions to explore

1. If you train an embedding model using cosine similarity as the training objective but then search with L2 (Euclidean) distance at query time, will your results be wrong? Why or why not? Think about what normalization does to the relationship between these two metrics.

2. Suppose you have a training batch of 512 query-document pairs. With in-batch negatives, how many total contrastive comparisons does each query see? Now imagine your batch size is only 16 -- what happens to training quality, and why?

3. A user complains: "I embedded my documents with Model A and my queries with Model B, and search results are terrible." Why would this fail even if both models produce 768-dimensional vectors and both were trained with cosine similarity?
