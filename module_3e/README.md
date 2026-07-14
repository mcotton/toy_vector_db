# Module 3E: Indexing — IVF (Inverted File Index) — Optional

**Why this module:** IVF is what production systems actually default to at scale — FAISS's flagship indexes are IVF variants. It's also the payoff for Module 3C: paired with PQ (as IVF-PQ), it's the architecture behind billion-scale vector search. And it's conceptually cheap — you already know every ingredient.

**When to take it:** Best right after Module 3C, while k-means is fresh. Works any time after. Skippable — Module 3D does not depend on it.

## Before You Start

Answer these readiness questions:
1. In your own words, what does k-means do? (You used it in Module 3C — where, exactly?)
2. Why did your KD-Tree fail in high dimensions? Specifically: what does a single axis-aligned split boundary test, and what does actual distance depend on?
3. Your PQ index from Module 3C compresses vectors 32x. When you search it, how many stored vectors do you compare the query against?
4. If you clustered your dataset into 100 groups and knew which group the query "belongs" to, how much work could you skip?

**#1 and #3 must be solid** — IVF reuses Module 3C's k-means directly, and #3 exposes the gap IVF fills: PQ makes each comparison cheap, but you're still comparing against *everything*. **#2 is the key contrast** — IVF partitions space like a KD-Tree tried to, but in a way that survives high dimensions. **#4 is the whole idea** — if you can answer it, you've already invented IVF.

## Introduction

Every index so far attacks a different problem: KD-Trees and LSH try to *look at fewer vectors*, PQ makes *each look cheaper*. But PQ alone still scans every code — you compressed the haystack without shrinking the search. IVF brings back the "look at fewer vectors" idea, using a partitioning scheme that — unlike KD-Trees — actually works in high dimensions.

**The idea:** cluster your dataset with k-means into `nlist` groups (say, 100). For each cluster, keep a list of the vectors assigned to it — an **inverted list** (the name comes from text search: an inverted file maps each word to the documents containing it; here it maps each centroid to the vectors nearest it). At query time, compare the query against the 100 centroids, pick the closest few clusters (`nprobe` of them), and brute-force search *only those lists*. With `nlist=100` and `nprobe=5`, you scan roughly 5% of the data.

**Why does this survive high dimensions when KD-Trees didn't?** A KD-Tree split tests *one dimension at a time* — and in 768 dimensions, one dimension tells you almost nothing about overall distance, so pruning fails and search backtracks everywhere. An IVF centroid comparison uses the *full* distance across all dimensions, and the cells are data-driven: k-means places centroids where the data actually lives (on its manifold), not on arbitrary axis-aligned grid lines. You partition by "which real cluster of data is this near," not "is coordinate 47 above or below the median."

**The catch — the edge problem.** The query's true nearest neighbor might sit just across a cell boundary, in a cluster whose centroid is slightly farther from the query. Probe only one cell and you miss it. This is why `nprobe` exists: probing several nearby cells recovers most boundary misses. Sound familiar? It's the same recall knob you keep meeting — LSH's L tables, and (in the next module) HNSW's ef_search. One important operational difference: `nprobe` is chosen *at query time*, no rebuild required.

**IVF-PQ — the composition.** IVF and PQ solve complementary problems, so production systems stack them: partition with IVF, then store PQ codes (instead of raw vectors) inside each inverted list. Search becomes: find `nprobe` nearest cells, then ADC-scan the compressed codes in just those lists. Fewer comparisons *and* cheaper comparisons *and* 30x less memory. That's how a billion vectors fit on one machine and get searched in milliseconds.

The parameters you'll tune:
- **nlist**: number of clusters. More = smaller lists (less scanning per probe) but more centroid comparisons and more boundary edges. Rule of thumb: around √n.
- **nprobe**: clusters searched per query. More = better recall, slower queries. This is your recall dial.

## Topics
- k-means as a coarse quantizer: partitioning by nearest centroid
- Inverted lists: centroid ID → vector IDs
- Search: rank centroids, probe top-`nprobe` lists, brute-force within
- The edge problem: why `nprobe=1` misses boundary neighbors
- nlist/nprobe trade-off; nlist ≈ √n rule of thumb
- IVF-PQ: PQ codes inside inverted lists (mention: production systems refine this by encoding residuals, vector − centroid, instead of raw vectors)

## Optional: Visualizing the Cells

**Goal:** See the partition, and see exactly how a true nearest neighbor gets missed.

**Exercise:** Generate 500 random 2D points (add a few gaussian clusters so k-means has structure to find). Run k-means with nlist=16 and plot the Voronoi cells with points colored by cluster. Pick a query point near a cell boundary. Highlight: the cell searched at nprobe=1, the cells added at nprobe=2 and 4, the true nearest neighbor. Find a query where the true nearest neighbor is *not* in the nprobe=1 cell — that's the edge problem, live.

**Key insight:** Misses concentrate near boundaries, and probing neighboring cells recovers them at linear cost. Compare this picture mentally with your Module 3A KD-Tree plot: both partition space, but these cells follow the data.

If you'd rather skip the visualization, ask your instructor to generate the plots for you.

## Deliverable
Implement IVF: build with `nlist` k-means clusters, search with configurable `nprobe`. Measure recall@10 vs. `nprobe` (1, 2, 4, 8, 16) at fixed nlist. Then compose IVF-PQ: store PQ codes in the lists, and compare memory and recall against plain IVF and against your Module 3C flat PQ scan.

**Grading:** your instructor writes and runs a test suite against this deliverable — it must pass, along with all previous modules' suites (see `OVERVIEW.md` → Verification and Grading).

## Checkpoint Questions
- A query's true nearest neighbor is in a cluster you didn't probe. How did that happen, which parameter fixes it, and what does the fix cost?
- 1M vectors, nlist=1000, nprobe=10: roughly how many distance computations per query (centroids + list scans)? What fraction of brute force is that?
- IVF's `nprobe`, LSH's `L`, and HNSW's `ef_search` all trade speed for recall. What makes `nprobe` and `ef_search` operationally friendlier than `L`?
- Why does searching PQ codes inside IVF lists lose slightly more recall than IVF over raw vectors — and why is that usually a good trade?
