# Module 2: Storage and Persistence

**Why this matters:** A database that forgets everything on restart isn't a database.

## Topics
1. Memory layout: Python lists vs. NumPy contiguous arrays — why it matters
2. Serialization options: pickle, JSON, custom binary, NumPy's `.npy`/`.npz`
3. Saving and loading the vector store to/from disk
4. What to persist: vectors, metadata, index structures

## Deliverable
Add `save(path)` and `load(path)` methods to the VectorStore. Choose a serialization format and justify the choice.

## Checkpoint Questions
- Why is `numpy.ndarray` of shape `(n, d)` faster to search than `list[list[float]]`?
- What happens if you pickle a 1GB array? What are the alternatives?
- When would you choose JSON over binary for storing vectors?
