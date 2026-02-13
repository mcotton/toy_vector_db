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

You've built four index types, each with different trade-offs. But how much faster are they actually? How much recall do they sacrifice? How much memory do they use? Without rigorous measurement, you're guessing.

This module turns your intuitions into data. You'll build a benchmark harness that measures three things for each index:
- **Recall@k**: what fraction of true nearest neighbors does the index find?
- **Queries per second (QPS)**: how fast is search?
- **Memory usage**: how much RAM does the index consume?

You'll produce trade-off curves (recall vs. speed, recall vs. memory) and compare your implementations against production libraries like FAISS or hnswlib. Some of your indexes will look good. Others will be embarrassingly slow. Both outcomes are informative.

Good benchmarking is a skill in itself. You'll learn about warm-up runs, statistical variance, dataset selection, and how to avoid common pitfalls (like measuring build time as part of query time, or using unrealistically small datasets).

## Topics
1. Recall@k measurement (compare ANN results to brute-force ground truth)
2. Queries per second
3. Memory profiling
4. Speed/recall trade-off curves
5. Comparison against FAISS or hnswlib

## Deliverable
A benchmark script that produces a table: index type, recall@10, QPS, memory usage.
