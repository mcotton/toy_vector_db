# Module 5: Interface Design

**Why this matters:** Even a library needs a clean, usable API.

## Before You Start

Answer these readiness questions:
1. Look at your current VectorStore code. If someone else had to use it, what would confuse them? What's missing?
2. Have you used a database library in Python (e.g., SQLite, SQLAlchemy, Redis client)? What made its API easy or hard to use?
3. What happens if a user inserts a 768-dim vector into a store that already has 384-dim vectors? Does your current code handle this?
4. If a user wanted to store two separate collections (e.g., "products" and "documents") with different dimensions and different index types, how would they do it with your current design?

**#1 and #3** will reveal gaps in your current implementation. **#2** gives you reference points for API design. **#4** motivates the core deliverable: collection management.

## Introduction

You've built distance functions, brute-force search, persistence, and multiple index types. But the code is scattered across modules with inconsistent interfaces. A user would need to know which file to import, how to configure each index, and how to manage multiple vector spaces.

This module is about turning your collection of implementations into a cohesive library. You'll design a `VectorDB` class that manages named collections, each with its own dimension, distance metric, and index backend. You'll add input validation so users get clear errors instead of cryptic NumPy exceptions. You'll add batch insert so users don't pay per-vector overhead.

This isn't glamorous work, but it's what separates a learning exercise from something actually usable. The decisions you make here (naming, defaults, error messages, what to expose vs. hide) are the same decisions library authors face.

## Topics
1. Collection management (multiple named vector spaces)
2. Batch insert for performance
3. Input validation and error handling
4. Choosing which index to use per collection

## Optional: API Comparison Study

**Goal:** Learn from production vector databases before designing your own API.

**Exercise:** Pick 2-3 production vector databases (e.g., Qdrant, Pinecone, ChromaDB, Weaviate) and look at their Python client APIs. For each, find the code to: create a collection, insert vectors, search, and filter by metadata. Compare:
- How many lines of code does each operation take?
- What do they name their methods? (`insert` vs `upsert` vs `add`?)
- How do they handle configuration (distance metric, dimensions, index type)?
- What do error messages look like when you pass wrong dimensions?

Write down 3-5 design decisions you'd steal for your own API, and 1-2 things you'd do differently.

**Key insight:** Good APIs are consistent, hard to misuse, and give clear errors. Bad APIs require reading source code to understand. Studying existing designs is faster than inventing from scratch.

If you'd rather skip this research and dive straight into coding, that's fine — but expect to redesign your API at least once.

## Deliverable
Refactor into a clean `VectorDB` class with collection support and configurable index backend.
