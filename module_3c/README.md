# Module 3C: Indexing — Product Quantization (PQ)

**Why third:** Shifts focus from "search faster" to "use less memory." Critical concept for scale.

## Topics
- Subvector decomposition: splitting a d-dimensional vector into m subvectors
- Learning codebooks via k-means on each subspace
- Encoding vectors as sequences of centroid IDs
- Asymmetric distance computation (ADC)
- Compression ratio: from `d * 4 bytes` to `m * 1 byte`

## Deliverable
Implement PQ encoding and asymmetric distance search. Measure memory savings and recall degradation vs. exact search.

## Checkpoint Questions
- If d=768 and m=96, how many subvectors? What dimension is each?
- Why asymmetric distance (query is not quantized) vs. symmetric (both quantized)?
- What's the trade-off of more subvectors (larger m)?
