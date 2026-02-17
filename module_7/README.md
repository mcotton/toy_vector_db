# Module 7: Benchmarking and Evaluation

**Why this matters:** Prove your indexes actually work. Or prove they don't and understand why.

## Before You Start

Answer these readiness questions:
1. What is recall@k? If a brute-force search returns the true 10 nearest neighbors, and your ANN index returns 8 of those 10, what is recall@10?
2. How would you measure "queries per second"? What could make that measurement unreliable?
3. Have you used Python's `time` module or `timeit` for benchmarking? Do you know why you should run multiple iterations?
4. How would you measure how much memory your index uses? (Rough ideas are fine.)

**#1 is critical** — recall is the primary quality metric for approximate search. If you can't define it, read up before starting. **#2 and #3** are about measurement methodology — getting reliable numbers is harder than it sounds. **#4** will be taught here, but any prior exposure to `sys.getsizeof` or memory profilers helps.

## Introduction

You've built four index types, each with different trade-offs. But how much faster are they actually? How much recall do they sacrifice? How much memory do they use? So far you've had rough impressions — "the KD-Tree was slower in high dimensions" — but no precise numbers. Without rigorous measurement, you're guessing.

**Why is benchmarking hard?** It sounds simple — run a search, time it, done. But measurements can be misleading. Your first query might be slow because data isn't cached yet; subsequent queries might be fast because the OS cached it in memory. One run might be slow because another program was using the CPU. Small datasets might make an index look good, but at 10x scale it falls apart. Good benchmarking accounts for these factors.

**What are the key metrics?**

- **Recall@k**: the quality metric. Run the same query with brute force (the "ground truth") and with your index. If brute force returns 10 true nearest neighbors and your index finds 8 of them, recall@10 = 0.8. Higher is better; 1.0 means the approximate index found the exact same results as brute force.
- **Queries per second (QPS)**: the speed metric. How many search queries can your index answer in one second? Higher is better.
- **Memory usage**: the cost metric. How much RAM does the index consume? Less is better.

The interesting part: these three metrics are in tension. Higher recall usually means slower queries and more memory. You'll produce trade-off curves showing exactly how these trade-offs play out for each of your indexes.

You'll also compare your implementations against production libraries like FAISS or hnswlib. Some of your indexes will look good. Others will be embarrassingly slow. Both outcomes are informative — the gap between your implementation and a production library reveals what optimizations matter most.

## Topics
1. Recall@k measurement (compare ANN results to brute-force ground truth)
2. Queries per second
3. Memory profiling
4. Speed/recall trade-off curves
5. Comparison against FAISS or hnswlib

## Deliverable
A benchmark script that produces a table: index type, recall@10, QPS, memory usage.
