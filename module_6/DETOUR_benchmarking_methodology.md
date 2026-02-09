# Detour: How to Benchmark Honestly

*Optional deep-dive between Module 6 (Real Embeddings) and Module 7 (Benchmarking and Evaluation)*

---

## Why this detour?

You're about to benchmark your vector DB. Before you write a single timing measurement, you need to understand how benchmarks lie -- because most of them do. Good benchmarking methodology is the difference between "my system is fast" (meaningless) and "my system achieves 95% recall@10 at 5000 QPS on this specific dataset" (useful).

---

## Topics

- **ann-benchmarks.com methodology.** The gold standard for ANN benchmarks. It measures recall@k vs queries-per-second across standardized datasets (SIFT1M, GloVe, etc.). The key insight: it plots a Pareto frontier, not a single number. Every system can trade speed for recall by tuning parameters. The question is which system gives you the best trade-off curve.

- **Why synthetic benchmarks lie.** Random uniform vectors behave differently from real embeddings. Real data has clusters, outliers, and non-uniform density. An index that performs well on uniform data might struggle on clustered data (or vice versa). Always benchmark on data that resembles your actual workload.

- **Recall@k is the gold standard.** Not precision, not F1, not "average distance to true neighbors." Recall@k answers: "of the true k nearest neighbors, how many did my approximate search actually find?" It's the metric that directly measures whether your users are seeing the right results.

- **The problem with QPS numbers.** Queries-per-second depends on batch size (batching amortizes overhead), parallelism (multi-threaded vs single-threaded), whether you include index loading time, and whether the data fits in cache. A QPS number without these details is meaningless. Always report the conditions.

- **Benchmark what matters to your users.** Latency distribution (p50, p95, p99) matters more than average latency. Throughput under concurrent load matters more than single-query speed. Recall with filters matters more than recall without. Build benchmarks that match your actual deployment scenario.

---

## Questions to explore

1. Your system achieves 99% recall@10 on random vectors but only 85% on your actual embedded documents. What could explain this gap? What property of your real data might be causing the index to struggle?

2. System A achieves 10,000 QPS at 95% recall. System B achieves 5,000 QPS at 99% recall. Which is better? The answer is "it depends" -- on what?

3. You benchmark your HNSW implementation and find that the first query after loading the index is 10x slower than subsequent queries. Why? Should you include this in your benchmark numbers?
