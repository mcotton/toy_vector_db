# Module 0: Foundations — Vectors, Distance, and Similarity

**Why this matters:** Everything in a vector DB depends on measuring how "close" two vectors are. If you don't understand this, nothing else will make sense.

## Before You Start

This module assumes almost no math background — just basic algebra (addition, multiplication, square roots). You'll build up from there.

Answer these to gauge your starting point (no wrong answers — this helps your instructor calibrate):
1. What is a vector? Can you give an example?
2. Have you ever used a vector database or semantic search tool (e.g., Pinecone, Weaviate, pgvector, or a RAG pipeline)? What was your experience?
3. What does "similarity" mean to you intuitively? If someone said two things are "close" in a database, what would that mean?

If you can answer #1 with something like "an array of numbers" and have any intuition at all for #3, you're ready. If you've used vector search as a consumer (#2), even better — this module will show you what's happening under the hood.

## Introduction

Modern AI systems (ChatGPT, image search, recommendation engines) convert data — text, images, audio — into arrays of numbers called **embeddings**. These embeddings live in high-dimensional space, and their positions encode meaning: similar concepts end up near each other.

A vector database stores these embeddings and answers one fundamental question: **given a new vector, which stored vectors are most similar to it?**

To answer that question, you need to measure distance. This module teaches you how, starting from first principles. You'll implement four different distance metrics by hand, understand when each is appropriate, and discover why high-dimensional space behaves in unintuitive ways.

## Topics
1. What is a vector in this context? (data point in high-dimensional space, not physics)
2. Embeddings: how text/images become vectors, why similar meanings produce nearby vectors
3. Dimensionality: what it means for a vector to have 384 vs 1536 dimensions
4. Distance metrics (implement all, understand trade-offs):
   - Euclidean (L2) distance
   - Cosine similarity
   - Dot product
   - Manhattan (L1) distance
   - When each is appropriate and why
5. The curse of dimensionality — why everything gets hard in high dimensions

## Optional: Visualizing Distance Metrics

**Goal:** Build geometric intuition for how different distance metrics measure "closeness" differently.

**Exercise:** Plot a reference point at the origin in 2D and draw the "unit circle" for each metric — the set of all points at distance=1 from the origin. Euclidean gives a circle, Manhattan gives a diamond, and cosine gives... what shape? Think about why before plotting.

Then place 3-4 labeled points in 2D and compute all four distances to a query point. Rank the points by each metric. Do the rankings differ? When would they agree and when would they disagree?

**Key insight:** Different metrics define "closest" differently. Two points can be nearest neighbors under cosine but not under Euclidean. This isn't a bug — it reflects what each metric cares about (direction vs. position).

If you'd rather skip the visualization and move on to implementation, ask your instructor to generate the plots for you. The goal is geometric intuition, not matplotlib skills.

## Deliverable
Implement all four distance functions from scratch (pure Python loops). Then rewrite with NumPy. Benchmark both. Explain the performance gap.

## Checkpoint Questions
- Compute cosine similarity of `[1, 2, 3]` and `[4, 5, 6]` by hand
- When do cosine similarity and dot product give the same nearest-neighbor ranking?
- Why does Euclidean distance become less meaningful as dimensions increase?
