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

Your Module 1 VectorStore works, but it has two problems: (1) it loses all data when the process ends, and (2) it's slower than it needs to be because of how it stores data in memory.

**The memory layout problem:** Right now your vectors are stored as separate Python objects — a list of individual arrays scattered across memory. When your CPU needs to process them one after another, it has to jump to a different memory location for each vector. This is like reading a book where every page is in a different room — you spend most of your time walking between rooms instead of reading.

A **contiguous array** stores all vectors packed together in one block of memory. The CPU can read through them sequentially without jumping around, and NumPy can process the entire block in a single operation (called **vectorization**) instead of looping through vectors one at a time in Python. This alone can make search 100-300x faster.

**The persistence problem:** When your Python process exits, everything in memory disappears. **Serialization** is the process of converting in-memory data into bytes that can be saved to a file and loaded back later. Think of it like saving a game — you need a format that captures the full state and can reconstruct it. There are multiple ways to serialize data (JSON, pickle, NumPy's binary format), each with different trade-offs in speed, file size, and compatibility.

This module solves both problems. You'll refactor storage to use a single contiguous NumPy array, then add save/load so your database survives restarts.

## Topics
1. Memory layout: Python lists vs. NumPy contiguous arrays — why it matters
2. Serialization options: pickle, JSON, custom binary, NumPy's `.npy`/`.npz`
3. Saving and loading the vector store to/from disk
4. What to persist: vectors, metadata, index structures

## Optional: Visualizing the Vectorization Speedup

**Goal:** See why contiguous memory and vectorized operations matter, not just hear that they're faster.

**Exercise:** Benchmark three approaches to computing distances between a query and n stored vectors:
1. Python loop over a list of Python lists
2. Python loop over a list of NumPy arrays
3. Single vectorized NumPy operation on a contiguous `(n, d)` array

Plot n (100 to 100,000) vs. query time for all three on the same chart. The gap between approach 1 and approach 3 should be dramatic (100-300x).

**Key insight:** The speedup isn't just "NumPy is faster." It's about memory layout. A contiguous array lets the CPU load sequential data into cache lines efficiently. A list of separate objects forces the CPU to chase pointers across memory. Vectorized operations also push the loop into C, avoiding Python's per-element overhead.

If you'd rather skip the visualization, ask your instructor to generate the benchmark script and chart for you.

## Deliverable
Add `save(path)` and `load(path)` methods to the VectorStore. Choose a serialization format and justify the choice.

**Grading:** your instructor writes and runs a test suite against this deliverable (including a save → load → search round-trip) — it must pass, along with all previous modules' suites (see `OVERVIEW.md` → Verification and Grading).

## Checkpoint Questions
- Why is `numpy.ndarray` of shape `(n, d)` faster to search than `list[list[float]]`?
- What happens if you pickle a 1GB array? What are the alternatives?
- When would you choose JSON over binary for storing vectors?
