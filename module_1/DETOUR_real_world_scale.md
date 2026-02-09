# Detour: When Brute Force Meets Reality

*Optional deep-dive between Module 1 (Brute-Force Search) and Module 2 (Storage and Persistence)*

---

## Why this detour?

You just built brute-force search and it works. For a few thousand vectors, it's fast enough that you might wonder why anyone bothers with anything else. This detour is about developing an intuition for where that breaks down -- not in theory, but in the actual systems companies run in production.

---

## Topics

- **Real-world dataset sizes are staggering.** Spotify embeds ~100 million tracks. Google embeds billions of web pages. A RAG pipeline might have 10-50 million document chunks. At 768 dimensions and 4 bytes per float, one million vectors is ~3 GB. One billion is ~3 TB. Just storing the vectors is a problem, let alone searching them.

- **Latency requirements are brutal.** Users expect search results in under 100ms. Many production systems target p99 latency under 50ms. Brute-force over 1 million 768-dim vectors takes 50-200ms on a single core. At 100 million vectors, you're looking at seconds per query.

- **What happens when data doesn't fit in RAM.** Every query hits disk. A single random SSD read takes ~100 microseconds vs ~100 nanoseconds for RAM -- a 1000x difference. Brute-force search that touches every vector becomes catastrophically slow.

- **The numbers that should scare you.** Brute-force is O(n * d) per query. At 1 billion vectors and 768 dimensions, that's 768 billion floating-point operations per query. Even at 100 GFLOPS (a fast CPU), that's ~8 seconds per query with zero concurrency.

---

## Questions to explore

1. You have 10 million vectors at 384 dimensions, stored as float32. How much RAM does just the vector data require? If your server has 32 GB of RAM, is brute force feasible? Show the math.

2. A product manager says "just shard the data across 10 servers and search in parallel." What's the best-case latency now? What new problems does this introduce?

3. Approximate nearest neighbor algorithms guarantee, say, 95% recall. Under what circumstances would you accept that 5% miss rate? Under what circumstances would you refuse it?
