# Detour: Why Spatial Partitioning Hits a Wall

*Optional deep-dive between Module 3A (KD-Trees) and Module 3B (LSH)*

---

## Why this detour?

You just built a tree-based index and saw it struggle as dimensions increased. This detour digs into why that happens at a fundamental level -- not just "the curse of dimensionality" as a buzzword, but the geometric reality that makes partition-based methods degrade. Understanding this makes it clear why the field moved toward probabilistic methods.

---

## Topics

- **The backtracking problem is the real killer.** In low dimensions, a KD-tree prunes most of the search space. In high dimensions, the query point is close to almost every partition boundary. The search backtracks across boundaries constantly. By 20-30 dimensions, the tree visits so many nodes it's barely faster than brute force.

- **Annoy (Spotify) -- a tree approach that partially works.** Builds a forest of random projection trees. Instead of axis-aligned splits (KD-tree), it picks two random points and splits on the hyperplane between them. Multiple trees + union of candidates mitigates the worst problems but doesn't eliminate them. Still widely used for medium-scale problems.

- **Random projections and Johnson-Lindenstrauss.** A deep mathematical result says you can project high-dimensional points into lower dimensions while approximately preserving distances. Random projections exploit this. This connects directly to why LSH works.

- **The philosophical shift.** Tree methods try to deterministically partition space for exact answers. When that fails in high dimensions, the field made a conceptual leap: stop guaranteeing exact answers, and instead design methods where you'll *probably* find a good answer. From deterministic to probabilistic -- one of the most important ideas in the field.

---

## Questions to explore

1. You build a KD-tree for 128-dim vectors and queries visit 85% of all nodes. Someone suggests increasing leaf size to 1000 and doing brute force within each leaf. Does this help? Why or why not?

2. Annoy builds multiple trees to compensate for splits separating true neighbors. Is there a fundamental limit to how much recall you can recover by adding more trees?

3. If high-dimensional space makes "everything approximately equidistant," why does vector search work at all? Real embeddings genuinely place similar items closer. What resolves this contradiction?
