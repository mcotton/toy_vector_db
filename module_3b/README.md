# Module 3B: Indexing — Locality-Sensitive Hashing (LSH)

**Why second:** Introduces the probabilistic/approximate mindset. Totally different paradigm from tree-based.

## Topics
- Random hyperplane hashing (for cosine similarity)
- Hash collisions as a feature, not a bug
- Multiple hash tables for better recall
- Tuning: number of hash functions vs. number of tables

## Deliverable
Implement an LSH index using random hyperplane projections. Measure recall@10 as you vary the number of hash tables and hash bits.

## Checkpoint Questions
- Why does increasing the number of hash functions decrease recall but increase precision?
- Why does increasing the number of tables increase recall?
- What's the space complexity of LSH with L tables and K hash functions?
