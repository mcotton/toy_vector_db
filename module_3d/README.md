# Module 3D: Indexing — HNSW (Hierarchical Navigable Small World)

**Why last:** The most important algorithm in modern vector search. Also the hardest. Everything before this builds toward understanding it.

## Topics
- Small-world graphs: short paths between any two nodes
- Navigable small-world: greedy routing works
- The NSW construction algorithm: insert nodes, connect to nearest neighbors
- Adding hierarchy: multiple layers (skip-list analogy)
- Search: start at top layer, greedy descend, beam search at bottom
- Parameters: M (max connections), ef_construction (build quality), ef_search (search quality)

## Deliverable
Implement HNSW from scratch. Benchmark recall@10 and queries/second against brute force, KD-Tree, and LSH.

## Checkpoint Questions
- Why does HNSW need multiple layers? What would happen with only one layer?
- How does the parameter M affect recall, speed, and memory?
- Why is HNSW construction not easily parallelizable?
- What happens to search quality when you delete nodes from the graph?
