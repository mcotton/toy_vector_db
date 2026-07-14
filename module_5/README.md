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

## The API Contract

Your library must expose at least this surface. Names can vary if you have a reason — behavior can't.

```python
db = VectorDB()

col = db.create_collection("products", dim=384, metric="cosine", index="brute_force")
db.list_collections()            # -> ["products"]
db.get_collection("products")    # -> the Collection
db.delete_collection("products")

col.insert(vector, metadata={"category": "electronics"})   # -> assigned id
col.insert_batch(vectors, metadatas)                       # -> [ids]
col.search(query, k=5, filters={"category": "electronics"})
# -> [{"id": ..., "score": ..., "metadata": {...}}, ...]
```

Design rules — each one is a bug you'll otherwise ship:

1. **Validate at the boundary.** A wrong-dimension insert or query raises `ValueError` naming expected vs. got — never a NumPy broadcast error three frames deep. An unknown metric or index name raises immediately and lists the valid options.
2. **Namespace your response.** User metadata goes under a `metadata` key, never merged flat into the result dict. Ask yourself what happens if a user's metadata contains the key `"id"` — your API must make that collision impossible, not just unlikely.
3. **Say what the number means.** Cosine similarity ranks descending; Euclidean distance ranks ascending. Pick one convention for `score` (e.g., "higher is always better") or name the field per metric — but a user must never have to read your source to learn which end is "best." Results must arrive best-first, both metrics.
4. **Don't touch the caller's data.** Never mutate a metadata dict the user passed in — and never use a mutable default argument (`def insert(self, v, metadata={})` is a famous Python bug; look it up if you haven't hit it yet).
5. **Index choice is per-collection, fixed at creation.** Switching index type on live data means a rebuild — make the user create a new collection rather than pretending it's free.
6. **Internal state stays internal.** If your store batches pending inserts or rebuilds indexes lazily, a search immediately after an insert must still see that insert. The user never calls `rebuild_*` anything.

## Acceptance Script

Your module is graded against a usage script that must run top to bottom without edits. It will, at minimum:
- Create two collections with different dims, metrics, and index backends
- `insert` and `insert_batch`, then search each collection — results best-first, exactly k of them when k matches are available
- Run a filtered search where the filter matches fewer than k items — verify count and contents
- Insert, then immediately search (no explicit commit/rebuild) — the new vector must be findable, including through filters
- Deliberately trigger each validation rule (wrong-dim insert, wrong-dim query, unknown metric, search on a deleted collection) and check the error message quality

Write your own version of this script as you build. If running it feels tedious, that's the API telling you something.

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
Refactor into a `VectorDB` class satisfying the API contract above: collection support, configurable index backend, validation with clear errors, and the acceptance script running clean.

**Grading:** your instructor writes and runs a test suite (including the acceptance script) against this deliverable — it must pass, along with all previous modules' suites (see `OVERVIEW.md` → Verification and Grading).

## Checkpoint Questions
- A user's metadata contains the key `"id"`. What does your search response return, and what in your API design makes the collision impossible?
- Your search returns a number per result. Is bigger better or worse — and how does a user of your library know without reading the source?
- Show the exact error message your library produces when a 384-dim vector is inserted into a 768-dim collection. Why is failing at the boundary better than letting NumPy fail inside search?
- Why does `create_collection` take the index type upfront instead of letting users switch it later? What would switching actually require?
