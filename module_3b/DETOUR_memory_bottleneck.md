# Detour: When Memory Is the Real Bottleneck

*Optional deep-dive between Module 3B (LSH) and Module 3C (Product Quantization)*

---

## Why this detour?

So far you've focused on making search faster -- pruning comparisons. But there's another wall: memory. At production scale, just storing the vectors can require terabytes. This detour forces you to confront the memory math directly and understand why compression isn't optional.

---

## Topics

- **The memory math is unforgiving.** 1 billion vectors at 768 dimensions, float32: 1B * 768 * 4 = ~3 TB. A high-end server has 256-512 GB of RAM. You're off by an order of magnitude. And that's just raw vectors -- indexes and metadata need memory too.

- **You can't just buy more RAM.** RAM costs ~$5-10/GB at server scale, maxes out at 1-2 TB per machine, and doesn't scale linearly. Distributed systems help but introduce latency and complexity. At some point, compressing data is cheaper than adding hardware.

- **The quantization trade-off spectrum.** Binary quantization (1 bit/dim): 32x compression, significant recall loss. Scalar quantization (int8, 1 byte/dim): 4x compression, minimal recall loss. Product quantization (8-64 bytes/vector regardless of dimension): 12-96x compression, moderate recall loss. No free compression.

- **Compression is foundational, not optional.** In a traditional database, compression is an optimization. In a vector database, it's a design constraint. The compressed representation is what you search over. The algorithm must be designed around compressed vectors from the start.

---

## Questions to explore

1. 100 million vectors at 384 dimensions, float32. Calculate raw memory. Then with scalar quantization (int8) and product quantization (64 bytes/vector). For a 64 GB server, which options fit?

2. Why use product quantization when scalar quantization is simpler and loses less per dimension? What does PQ buy that SQ can't?

3. A colleague proposes storing vectors on SSD with memory-mapping. Under what workload does this work? When does it fall apart?
