# Toy Vector Database — Learning Plan

## Context
This is a learning project to build a vector database from scratch in Python. The goal is to deeply understand the data structures, algorithms, and design decisions behind vector search systems — not to produce a production-ready product.

**Teaching approach:** Socratic — ask questions, demand explanations, challenge understanding. No hand-holding. The learner writes the code; the instructor guides, questions, and critiques.

**Decisions locked in:**
- Language: Python
- Storage: In-memory + simple file save/load
- Interface: Python library only (importable module)
- Indexing: All four strategies, sequenced for learning (KD-Tree → LSH → PQ → HNSW)

---

## Progression Order
```
Module 0 (Foundations) ──mandatory──▶ Module 1 (Brute Force) ──mandatory──▶
Module 2 (Storage) ──▶ Module 3A (KD-Tree) ──▶ Module 3B (LSH) ──▶
Module 3C (PQ) ──▶ Module 3D (HNSW) ──▶ Module 4 (Filtering) ──▶
Module 5 (Interface) ──▶ Module 6 (Embeddings) ──▶ Module 7 (Benchmarks)
```

## Modules

| Module | Directory | Topic |
|--------|-----------|-------|
| 0 | `module_0/` | Foundations — Vectors, Distance, and Similarity |
| 1 | `module_1/` | Brute-Force Search (The Baseline) |
| 2 | `module_2/` | Storage and Persistence |
| 3A | `module_3a/` | Indexing — KD-Trees |
| 3B | `module_3b/` | Indexing — Locality-Sensitive Hashing (LSH) |
| 3C | `module_3c/` | Indexing — Product Quantization (PQ) |
| 3D | `module_3d/` | Indexing — HNSW |
| 4 | `module_4/` | Filtering and Metadata |
| 5 | `module_5/` | Interface Design |
| 6 | `module_6/` | Connecting to Real Embeddings |
| 7 | `module_7/` | Benchmarking and Evaluation |
| 8 | `module_8/` | Advanced Topics (Optional) |

## Optional Detours

Between each module there is an optional deep-dive that bridges concepts and provides real-world context. These are discussion-based (no code) and can be taken or skipped.

| After Module | Detour File | Topic |
|---|---|---|
| 0 → 1 | `module_0/DETOUR_embedding_training.md` | How Embedding Models Are Trained |
| 1 → 2 | `module_1/DETOUR_real_world_scale.md` | When Brute Force Meets Reality |
| 2 → 3A | `module_2/DETOUR_index_landscape.md` | The Indexing Landscape — What Production Systems Use |
| 3A → 3B | `module_3a/DETOUR_why_trees_fail.md` | Why Spatial Partitioning Hits a Wall |
| 3B → 3C | `module_3b/DETOUR_memory_bottleneck.md` | When Memory Is the Real Bottleneck |
| 3C → 3D | `module_3c/DETOUR_graph_revolution.md` | How Graphs Conquered Vector Search |
| 3D → 4 | `module_3d/DETOUR_beyond_similarity.md` | Real Queries Are Never Just "Find Similar" |
| 4 → 5 | `module_4/DETOUR_api_patterns.md` | How Production Vector DBs Design Their APIs |
| 5 → 6 | `module_5/DETOUR_embedding_ecosystem.md` | The Embedding Model Ecosystem |
| 6 → 7 | `module_6/DETOUR_benchmarking_methodology.md` | How to Benchmark Honestly |

## How Each Session Works
1. Introduce the module's core concepts and ask questions to gauge understanding
2. Learner explains back — instructor challenges gaps
3. Learner writes the code — instructor reviews, critiques, and asks "why did you do it this way?"
4. Checkpoint: learner must pass the questions before moving on
5. If they can't pass, we go back. No moving forward with gaps.

## Verification
- Unit tests for correctness at each module
- Benchmark comparisons to prove indexes help (or expose when they don't)
- Final integration test: real embeddings, real queries, measured recall
