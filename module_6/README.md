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

Up until now, you've been searching over random vectors. The numbers were meaningless — useful for testing algorithms, but not for answering real questions. This module connects your vector database to real embedding models and real data.

You'll take actual text documents, convert them to embeddings using a pre-trained model, store them in your vector database, and search by meaning. Ask "What is photosynthesis?" and find documents about plants converting sunlight to energy — even if they never use the word "photosynthesis."

Then you'll build a RAG pipeline: retrieve relevant documents from your vector database, inject them as context into an LLM prompt, and generate an answer grounded in your data. This is the architecture behind ChatGPT plugins, enterprise search, and most production AI applications.

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
