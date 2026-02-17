# Module 6: Connecting to Real Embeddings

**Why this matters:** This is where theory meets practice. This is the RAG pipeline connection.

## Before You Start

Answer these readiness questions:
1. Have you used an embedding model or API before? (e.g., OpenAI's `text-embedding-ada-002`, sentence-transformers, Cohere embed?)
2. What dimensionality do common embedding models output? (Rough guesses are fine.)
3. Can you explain in your own words what a RAG (Retrieval-Augmented Generation) pipeline does?
4. If you embed the sentence "The cat sat on the mat" and "A feline rested on the rug" — would you expect them to be close or far apart in embedding space? Why?

**#1 determines setup**: if you've never used an embedding model, we'll walk through installation and first use. If you have, we'll move faster. **#2** connects to your earlier work — you've benchmarked at 768 dims, and that's a real model dimensionality. **#3** is the end goal — if you can't explain RAG yet, this module will make it concrete. **#4** tests your intuition about what embeddings capture.

## Introduction

Up until now, you've been searching over random vectors — lists of meaningless numbers, useful for testing algorithms but not for answering real questions. This module is where your vector database starts doing something useful.

**What is an embedding model?** An embedding model is a neural network that converts data (text, images, audio) into a vector. You give it a sentence like "The cat sat on the mat" and it returns something like `[0.23, -0.87, 0.41, ..., 0.15]` — a list of hundreds of numbers. The model has been trained so that inputs with similar meaning produce similar vectors. You don't need to understand how the model works internally — you'll use it as a black box via a library (sentence-transformers) or an API (OpenAI, Cohere).

**What is semantic search?** Traditional keyword search matches exact words — searching for "car" won't find documents about "automobiles." Semantic search matches meaning — the embedding model maps both "car" and "automobile" to nearby vectors, so a search for one finds the other. This is powerful but not magic: the model can make mistakes, and some queries are genuinely ambiguous.

**What is RAG?** RAG (Retrieval-Augmented Generation) is a pattern for making LLMs (like ChatGPT) answer questions using your data. The steps are: (1) take the user's question, (2) embed it as a vector, (3) search your vector database for relevant documents, (4) paste those documents into the LLM's prompt as context, (5) ask the LLM to answer based on that context. This is the architecture behind ChatGPT plugins, enterprise search, and most production AI applications. You'll build a working version in this module.

You'll also discover failure modes: when does semantic search return irrelevant results? What kinds of queries break it? Understanding these limitations is as important as understanding the successes.

## Topics
1. Using sentence-transformers or an API to generate real embeddings
2. Indexing real documents (not random vectors)
3. Semantic search: query by meaning
4. Observing failure modes: when does semantic search fail and why?
5. RAG pipeline: retrieve → augment → generate

## Optional: Visualizing Embedding Space

**Goal:** See how real embeddings organize by meaning, not by surface-level word overlap.

**Exercise:** Embed 30-50 short sentences from 3-4 distinct topics (e.g., cooking recipes, programming concepts, sports scores, weather descriptions). Use PCA or t-SNE to project the embeddings down to 2D. Plot them colored by topic.

Do the clusters separate cleanly? Find cases where sentences from different topics end up close together — why did the model think they were similar? Find cases where sentences from the same topic are far apart — what makes them different in embedding space?

**Extension:** Embed pairs of sentences that are semantically similar but lexically different ("The car is fast" / "The automobile has great speed") and pairs that are lexically similar but semantically different ("bank of the river" / "bank account"). Plot them. Does the model handle these correctly?

**Key insight:** Embeddings capture meaning, not words. But they're not perfect — the model's training data and architecture create blind spots. Understanding where embeddings fail is as valuable as understanding where they succeed, especially for building reliable RAG pipelines.

If you'd rather skip the visualization, ask your instructor to generate the plots for you.

## Deliverable
Build a working semantic search demo. Index a corpus, search with natural language, show results. Feed retrieved results to an LLM as context.
