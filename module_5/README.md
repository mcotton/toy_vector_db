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

You've built distance functions, brute-force search, persistence, and multiple index types. But right now, using your code looks something like: import from this file, call this function, manually create a NumPy array, remember to pass the right arguments in the right order. A user would need to read your source code to figure out how anything works. That's not a library — it's a pile of scripts.

**What is an API?** An API (Application Programming Interface) is the set of functions, classes, and methods that a user interacts with. Think of it like the controls on a car: the steering wheel, pedals, and gear shift are the API. The engine, transmission, and braking system are the implementation hidden underneath. A good API is easy to use correctly and hard to use wrong. A bad API requires reading the owner's manual for every action.

**What does "interface design" mean here?** You'll design a `VectorDB` class that wraps all your implementations behind a clean, consistent surface. A user should be able to create a collection, insert vectors, and search — without knowing whether the index underneath is brute force, LSH, or HNSW. You'll add input validation so mistakes produce helpful error messages ("Expected 768 dimensions, got 384") instead of cryptic NumPy crashes. You'll add batch insert so users can add thousands of vectors efficiently.

This isn't glamorous work, but it's what separates a learning exercise from something actually usable. The decisions you make here (naming, defaults, error messages, what to expose vs. hide) are the same decisions library authors face. If you've ever been frustrated by a confusing library, this is your chance to do better.

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
