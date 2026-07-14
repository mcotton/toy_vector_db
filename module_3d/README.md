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

You've now built several index types:
- **KD-Trees** partition space but fail in high dimensions (can't prune)
- **LSH** works in high dimensions but needs many tables for good recall (memory-expensive, recall is probabilistic)
- **PQ** compresses vectors but loses precision (lossy)
- **IVF** (if you took Module 3E) partitions with data-driven clusters, but recall still leaks at cell boundaries — `nprobe` recovers it only by scanning more cells

HNSW takes yet another approach, and it's the one that won. It's what most production vector databases (Qdrant, Weaviate, pgvector) use today. The idea: build a **graph** and walk through it.

**What is a graph?** In computer science, a graph is a set of **nodes** (points) connected by **edges** (links). Think of a social network: each person is a node, and friendships are edges. Or a road map: cities are nodes, roads are edges. In HNSW, each vector is a node, and edges connect it to its nearest neighbors. The graph captures the neighborhood structure of your data.

**What is greedy search?** A greedy algorithm makes the locally best choice at each step. Imagine you're lost in a city trying to reach a landmark you can see in the distance. A greedy approach: at every intersection, take the road that points most directly toward the landmark. This often works well but can fail — you might hit a dead end or a river with no bridge. In HNSW, greedy search means: at each node, look at all its neighbors and move to whichever one is closest to your query. Repeat until no neighbor is closer than where you are.

**The problem with a flat graph:** Greedy search can get stuck. Imagine trying to walk from one end of a large city to the other using only local streets — you'd take thousands of tiny steps, and if the street grid has gaps, you might end up in a dead end (a **local minimum**) where no neighbor is closer to your destination, but you haven't found the true nearest point.

**The solution is hierarchy.** HNSW builds multiple layers of the graph, like a transit system:
- **Top layers** have very few nodes spaced far apart — like an airline network connecting major cities. Greedy search here takes huge leaps across the dataset.
- **Middle layers** have more nodes — like a train network. More precise steps.
- **Bottom layer** has every node — like local streets. Search here is precise but slow.
- You start at the top and descend: each layer gets you closer, with increasing precision.

**The skip list analogy:** A **linked list** is a chain of items where each item points to the next — to find something, you walk the chain from the start. A **skip list** adds "express lanes" above the base chain: a top layer with every 16th item, then every 4th, then every item. Searching starts at the top (big jumps), drops down when you overshoot, and refines at the bottom. HNSW applies this same coarse-to-fine principle, but to graph-based search instead of a linear chain.

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

## Optional: Visualizing Graph Search

**Goal:** See how greedy graph search navigates toward a query, and understand why hierarchy prevents getting stuck.

**Exercise 1 — Flat NSW:** Generate 100 random 2D points. Build a simple navigable small-world graph: for each point, connect it to its M=4 nearest neighbors. Pick a query point and animate (or step through) greedy search: start at a random node, move to the neighbor closest to the query, repeat until no neighbor is closer. Plot the path taken. Does it find the true nearest neighbor? Try different starting points — does it sometimes get stuck?

**Exercise 2 — Adding hierarchy:** Build a 2-layer version. Layer 1 has ~10 randomly selected nodes (connected to each other). Layer 0 has all 100 nodes. Search layer 1 first (big leaps), then refine in layer 0 (precise steps). Plot both search paths. How does starting from the top layer change the result compared to starting from a random node in the bottom layer?

**Key insight:** In a flat graph, greedy search depends heavily on the starting point — a bad start means getting stuck in a local minimum. The hierarchy provides a good starting point by doing a coarse search first. This is the same principle as binary search (coarse to fine) applied to graph traversal.

If you'd rather skip the visualization, ask your instructor to generate the plots for you.

## Deliverable
Implement HNSW from scratch. Benchmark recall@10 and queries/second against brute force, KD-Tree, and LSH (and IVF, if you did Module 3E).

Then make it survive a restart: add save/load for the graph, or make the deliberate decision to rebuild on load and justify it (build time vs. file complexity vs. how often restarts happen). Module 2 promised "what to persist: index structures" — this is where that bill comes due.

**Grading:** your instructor writes and runs a test suite against this deliverable — it must pass, along with all previous modules' suites (see `OVERVIEW.md` → Verification and Grading).

## Checkpoint Questions
- Why does HNSW need multiple layers? What would happen with only one layer?
- How does the parameter M affect recall, speed, and memory?
- Why is HNSW construction not easily parallelizable?
- What happens to search quality when you delete nodes from the graph?
