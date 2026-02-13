# Module 2: Storage and Persistence

**Why this matters:** A database that forgets everything on restart isn't a database.

## Before You Start

Answer these readiness questions:
1. What does your VectorStore from Module 1 store in memory? What happens to it when your Python process exits?
2. Have you ever read/written files in Python? (e.g., `open()`, `json.dump()`, etc.)
3. Do you know the difference between a Python `list` and a NumPy `ndarray`? Even a vague sense counts.
4. What does "serialization" mean? (If you're unsure: it's converting in-memory data structures to bytes that can be saved to disk or sent over a network.)

**If #1 is unclear**, revisit your Module 1 VectorStore — understand what data it holds and how. **If #2 is new**, do a quick tutorial on Python file I/O first. **#3 and #4** will be taught in this module, but any prior exposure helps.

## Introduction

Your Module 1 VectorStore works, but it has two problems: (1) it loses all data when the process ends, and (2) it uses Python lists of separate NumPy arrays, which means scattered memory and no ability to use fast vectorized operations across the full dataset.

This module solves both. You'll refactor the internal storage to use a single **contiguous NumPy array** — which unlocks vectorized search (replacing your Python loop with a single matrix operation). Then you'll add save/load so your database persists to disk.

Along the way, you'll learn why memory layout matters for performance, evaluate serialization options (pickle, JSON, NumPy binary formats), and understand the trade-offs of each.

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
