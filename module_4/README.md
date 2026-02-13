# Module 4: Filtering and Metadata

**Why this matters:** Real queries are "find similar vectors WHERE category='X'", not just "find similar vectors."

## Before You Start

Answer these readiness questions:
1. In your VectorStore, each vector has a metadata dict. If a user wanted to search for similar vectors but only within a specific category, how would you do it with your current implementation?
2. Do you know what an inverted index is? (Think: how does a search engine find all documents containing a specific word?)
3. Have you used SQL WHERE clauses? The concept of filtering here is analogous.
4. Can you think of a scenario where filtering *before* search and filtering *after* search would give different results?

**#1 is the motivating problem** — try to think of a naive solution before reading further. **If #2 is new**, that's fine — it will be introduced here. **#3** is just for intuition. **#4** is the core insight of this module — if you can already see why the order matters, you're well-prepared.

## Introduction

Every real-world vector search includes constraints: "find similar products in the electronics category," "find similar documents written in the last year," "find similar images tagged as landscapes." Pure vector similarity isn't enough — you need to combine it with metadata filtering.

This sounds simple but creates a fundamental tension. There are two obvious approaches, and both are flawed:

- **Post-filtering**: run vector search as normal, get top-k results, then remove results that don't match the filter. Problem: you might filter out most of your results and return fewer than k.
- **Pre-filtering**: find all vectors matching the filter first, then search only within that subset. Problem: your ANN index was built over the *full* dataset — searching a subset may miss the true nearest neighbors or require a separate index per filter value.

Production systems use various strategies to handle this: over-fetching, hybrid indexes, partitioned indexes. You'll implement the simple approaches first and discover their limitations firsthand.

## Topics
1. Post-filtering: search first, filter results after
2. Pre-filtering: filter candidates, search within the subset
3. Over-fetching: request more than k results, filter, return top k
4. Metadata storage and inverted indexes

## Deliverable
Add metadata filtering to search: `search(query, k, filters={"category": "electronics"})`. Implement post-filtering first, then discuss why it's inadequate.

## Checkpoint Questions
- Why can post-filtering return fewer than k results?
- Why can pre-filtering miss true nearest neighbors?
- How do production systems handle this?
