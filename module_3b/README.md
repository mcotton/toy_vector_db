# Module 3B: Indexing — Locality-Sensitive Hashing (LSH)

**Why second:** Introduces the probabilistic/approximate mindset. Totally different paradigm from tree-based.

## Before You Start

Answer these readiness questions:
1. What happened when you ran your KD-Tree on 768-dimensional data? Why did it fail?
2. Do you know what a hash function is? Can you give an example of one? (Even `hash()` in Python or MD5 counts.)
3. Are you comfortable with the idea of an algorithm that gives the *right* answer most of the time but not always? What would you call that?
4. If two vectors have high cosine similarity, what does that mean geometrically about their directions?

**If #1 is unclear**, revisit Module 3A — understanding why tree-based partitioning fails in high dimensions is the motivation for LSH. **#2 is important** — if you've never used a hash function, read up on what hash tables are and how they work before starting. **#3** sets the mental shift: LSH is the first *approximate* algorithm in the course. **#4** connects to the specific LSH variant you'll build (random hyperplane hashing uses the angular relationship between vectors).

## Introduction

Module 3A taught you that spatial partitioning (trees) breaks down in high dimensions because you can't prune — every region might contain the nearest neighbor. LSH takes a completely different approach: instead of carefully organizing space, it uses **randomness** to group similar vectors together.

**What is a hash function?** A hash function takes an input and produces a fixed output (a "hash"). For example, Python's `hash("hello")` produces a number. Normal hash functions (like MD5 or SHA-256) are designed so that similar inputs produce very *different* outputs — "hello" and "hellp" get completely different hashes. This is good for hash tables and checksums, where you want even distribution.

**LSH flips this on its head.** A locality-sensitive hash function is designed so that similar inputs produce the *same* output. Two vectors that are close together should hash to the same value (land in the same bucket). Two vectors that are far apart should hash to different values. Hash collisions — normally a problem — are the entire point.

**How does it work?** The technique for cosine similarity uses random **hyperplanes**. A hyperplane is just a dividing surface: in 2D it's a line through the origin, in 3D it's a flat plane through the origin, in higher dimensions it's the same idea extended. Each hyperplane divides all of space into two halves. For any vector, you check which side it falls on — this is computed with a **dot product** (multiply corresponding elements and add them up). If the dot product is positive, the vector is on one side (hash bit = 1); if negative, the other side (hash bit = 0).

Stack multiple hyperplanes and each vector gets a multi-bit hash code like `10110`. Vectors pointing in similar directions end up on the same side of most hyperplanes, so they get similar (or identical) hash codes — and land in the same bucket.

The catch: a single set of random hyperplanes might separate two similar vectors by bad luck. The fix: use **multiple independent hash tables**, each with its own random hyperplanes. If two neighbors end up in the same bucket in *any* table, you find them. More tables = better recall, but more memory.

This creates a new set of trade-offs to explore: how many hash functions per table? How many tables? How do these parameters affect recall, precision, and memory?

## Topics
- Random hyperplane hashing (for cosine similarity)
- Hash collisions as a feature, not a bug
- Multiple hash tables for better recall
- Tuning: number of hash functions vs. number of tables

## Optional: Visualizing Hyperplane Hashing

Before implementing the full LSH index, it helps to *see* what random hyperplane hashing is doing. This is an optional visualization exercise using matplotlib.

**Exercise:** Generate 200 random 2D points, create K random hyperplanes, hash each point, and plot the points colored by bucket with the hyperplane lines drawn through the origin. Experiment with K=1, 2, 3, 4, 8, 50, 100.

**Key observation:** In 2D, K hyperplanes through the origin create at most **2K** regions (linear growth). But in d dimensions with d >= K, they can create up to **2^K** regions (exponential). This means LSH gets *more* useful as dimensions increase — the opposite of KD-Trees. The 2D visualization shows the mechanism correctly but can mislead about effectiveness at scale.

**Verification:** After the 2D plot, compare the number of occupied buckets for the same K across different dimensions (2D, 10D, 100D, 768D) to confirm that higher dimensions produce more distinct buckets.

If you'd rather skip this and move on to implementation, ask your instructor to generate the visualization code for you. The goal is understanding, not matplotlib skills.

## Deliverable
Implement an LSH index using random hyperplane projections. Measure recall@10 (defined in Module 3A) as you vary the number of hash tables and hash bits.

**Grading:** your instructor writes and runs a test suite against this deliverable — it must pass, along with all previous modules' suites (see `OVERVIEW.md` → Verification and Grading).

## Checkpoint Questions
- Why does increasing the number of hash functions decrease recall but increase precision?
- Why does increasing the number of tables increase recall?
- What's the space complexity of LSH with L tables and K hash functions?
