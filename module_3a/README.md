# Module 3A: Indexing — KD-Trees (Tree-Based Partitioning)

**Why first:** Intuitive spatial partitioning. Easy to visualize. Understanding why it fails in high dimensions is the key lesson.

## Before You Start

Answer these readiness questions:
1. Have you ever implemented a tree data structure (binary tree, BST, trie, etc.)? If not, that's OK — we'll build a basic binary tree first before tackling KD-Trees.
2. Are you comfortable with recursion? Can you write a function that calls itself with a smaller input and has a base case?
3. How does your current brute-force search work? What's its time complexity?
4. If you had a sorted list of numbers and needed to find one quickly, what approach would you use? What's its time complexity?

**If #1 is "no"**, plan for extra time — this module starts by building a basic binary tree from scratch to establish the fundamentals (nodes, left/right children, recursive construction, traversal) before extending to KD-Trees. **If #2 is shaky**, practice with a simple recursive function (factorial, fibonacci) first. **#3 should be solid** from Module 1 — you need the baseline to compare against. **#4** connects to the core idea: trees enable search by elimination.

## Introduction

Brute force compares your query against every stored vector. Can we do better? What if we could organize vectors spatially so we only need to check a fraction of them?

**The analogy:** Imagine you're looking for a specific house in a city. Brute force means knocking on every door. But if you know the house is in the northeast part of town, you can ignore the southwest entirely. That's the idea behind spatial partitioning — organize data by location so you can skip large regions that are far from what you're looking for.

**What is a tree?** A tree is a data structure where each item (called a node) can point to child items below it. A **binary tree** has at most two children per node (left and right). You start at the top (the root) and follow paths down to find what you need. If you've never built one, don't worry — this module includes scaffolding to build a basic binary tree first.

**What is recursion?** A recursive function calls itself with a smaller version of the problem. For example, to search a tree: check the current node, then recursively search the left subtree and the right subtree. Each call handles a smaller piece until you hit a base case (like reaching a node with no children). If this is new to you, practice with a simple example (like computing factorial: `5! = 5 * 4!`) before starting.

A **KD-Tree** (k-dimensional tree) applies binary tree thinking to multi-dimensional data. It recursively splits the vector space in half, alternating which dimension it splits on. Think of it like organizing a map: first divide by east/west, then within each half divide by north/south, then east/west again, and so on. When searching, you traverse the tree to find the region where your query falls, then check whether neighboring regions might contain closer points (this is called **backtracking**).

In low dimensions (2D, 3D), this is powerful — you might visit only 10-20 nodes out of thousands. But as dimensions increase, something goes wrong. Discovering *what* and *why* is the real lesson of this module. KD-Trees are historically important but fail in high dimensions, and understanding that failure motivates everything that comes after (LSH, PQ, HNSW).

**Measuring success — recall@k.** From this module on, every index you build gets judged against brute force. Brute force is exact, so its top-k results are the **ground truth**. Run the same query both ways and compare: **recall@k = (how many of the true top-k your index returned) / k**. If brute force says the 10 nearest neighbors are these, and your KD-Tree returns 8 of them, recall@10 = 0.8. A recall of 1.0 means your index found exactly what brute force found — just (hopefully) faster. You'll compute this metric in every remaining module, so build the habit now: benchmark = ground truth from brute force + recall + speed, never speed alone.

**Scaffolding for this module:**
1. Build a basic binary tree (if you haven't before) — Node class, recursive insert, display
2. Extend to KDNode with split dimensions — understand how 2D/3D space gets partitioned
3. Implement construction — recursive median split, alternating dimensions
4. Implement search — traversal first (visit every node), then add backtracking and pruning
5. Test at increasing dimensions — observe where it breaks down

## Topics
- Recursive binary space partitioning
- Choosing split dimensions and split values (median)
- Search: traversal + backtracking
- Why performance degrades exponentially with dimensions

## Optional: Visualizing KD-Tree Partitioning

**Goal:** See how a KD-Tree recursively divides 2D space and understand why nearby points can end up in different partitions.

**Exercise:** Generate 20-30 random 2D points and build a KD-Tree. Plot the points and draw the split lines: the root splits with a vertical line (split_dim=0), the next level splits with horizontal lines (split_dim=1), alternating at each depth. Color-code points by which leaf they belong to.

Then pick a query point, highlight its leaf region, and show which other regions the backtracking step would need to check (regions where the split boundary is closer than the current best distance). This visualizes why backtracking is necessary — the nearest neighbor can be across a split boundary.

**Key insight:** In 2D, most split boundaries are far enough from the query that you prune large regions. As you increase dimensions, the split boundaries get closer relative to inter-point distances (because each boundary only tests one dimension out of many). This is why pruning fails in high dimensions.

If you'd rather skip the visualization, ask your instructor to generate the plot for you.

## Deliverable
Implement a KD-Tree index. Run search on 3-dim, 100-dim, and 768-dim data. Compare recall@10 and speed against brute force.

**Grading:** your instructor writes and runs a test suite against this deliverable — it must pass, along with all previous modules' suites (see `OVERVIEW.md` → Verification and Grading).

## Implementation Notes
- **Build step uses `sorted()` instead of median selection.** This is a pedagogical simplification — sorting at each recursive level gives O(n log² n) build time instead of O(n log n) with linear-time median selection (e.g., `np.partition`). Standard KD-Tree implementations use the faster approach. Keep this in mind when analyzing build-time complexity or comparing benchmarks against reference implementations.

## Checkpoint Questions
- At what dimensionality does your KD-Tree become slower than brute force? Why?
- How does the tree depth relate to the number of points?
- What's the worst-case query time?
