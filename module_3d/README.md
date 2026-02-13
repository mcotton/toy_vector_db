# Module 3D: Indexing — HNSW (Hierarchical Navigable Small World)

**Why last:** The most important algorithm in modern vector search. Also the hardest. Everything before this builds toward understanding it.

## Before You Start

Answer these readiness questions:
1. Do you know what a graph is in computer science? (Nodes and edges, not bar charts.) Can you describe one?
2. Have you heard of a "greedy algorithm"? Can you explain the concept?
3. Have you seen or used a skip list? If not, do you know what a linked list is?
4. In your KD-Tree (Module 3A), what was the core problem in high dimensions? In LSH (Module 3B), what was the main limitation?

**If #1 is "no"**, spend some time with graph basics before starting — nodes, edges, neighbors, traversal. HNSW is fundamentally a graph algorithm. **#2 is important** because HNSW search is greedy: at each step, move to the neighbor closest to the query. **#3** helps with understanding the hierarchy — HNSW layers work like skip list levels. If you know linked lists, the skip list concept will be introduced here. **#4** should be solid — HNSW was designed to solve the problems you've already experienced.

## Introduction

You've now built three index types:
- **KD-Trees** partition space but fail in high dimensions (can't prune)
- **LSH** works in high dimensions but needs many tables for good recall (memory-expensive, recall is probabilistic)
- **PQ** compresses vectors but loses precision (lossy)

HNSW takes yet another approach: build a **graph** where each vector is a node connected to its nearest neighbors. To search, start at a random node and greedily walk toward the query — at each step, move to whichever neighbor is closest to what you're looking for.

The problem with a flat graph: greedy search gets stuck in local minima, especially in large datasets. You might find a "pretty good" neighbor but miss the actual nearest one because you'd need to traverse through distant nodes to reach it.

The solution is **hierarchy**. HNSW builds multiple layers of the graph:
- **Top layers** have very few nodes spaced far apart — like highway exits. Greedy search here takes huge leaps across the dataset.
- **Bottom layer** has every node — like local streets. Search here is precise but slow.
- You start at the top and descend: each layer gets you closer, with increasing precision.

This is analogous to a **skip list** — a data structure that adds "express lanes" over a linked list for O(log n) search. HNSW applies the same idea to graph-based nearest neighbor search.

The key parameters you'll tune:
- **M**: max connections per node (more = better recall, more memory)
- **ef_construction**: how many candidates to consider when building (more = better graph, slower build)
- **ef_search**: how many candidates to consider when searching (more = better recall, slower search)

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
