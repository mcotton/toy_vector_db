# Toy Vector DB — Build a Vector Database from Scratch

A hands-on learning course for building a vector database from scratch in Python. Designed to be worked through with Claude Code (or any AI tutor) using Socratic teaching — you write the code, the instructor guides and critiques.

## What You'll Learn

- How vectors and embeddings represent data in high-dimensional space
- Distance metrics (Euclidean, Cosine, Dot Product, Manhattan) and when to use each
- Brute-force kNN search as a baseline
- Four indexing strategies: KD-Trees, LSH, Product Quantization, HNSW (plus optional IVF)
- Metadata filtering, API design, and real-world embedding pipelines
- Benchmarking methodology for evaluating search systems

## Prerequisites

- Python experience (intermediate)
- Basic comfort with the command line
- No linear algebra background required (Module 0 covers what you need)

## Getting Started

1. Clone this repo
2. Create a personal branch: `git checkout -b my-progress`
3. Set up a Python venv: `python3 -m venv venv && source venv/bin/activate && pip install numpy`
   (Later modules add more — install as you reach them: matplotlib for the visualizations, scikit-learn for Modules 3C/3E, sentence-transformers for Module 6, hnswlib or faiss for Module 7, jupyter if you like notebooks.)
4. Read `OVERVIEW.md` for the full curriculum
5. Start with `module_0/README.md`

## Course Structure

Work through modules in order. Each module has:
- **README.md** — topics, deliverables, and checkpoint questions
- **DETOUR_*.md** (optional) — deep-dive topics bridging to the next module

Each module is **graded**: your instructor writes a test suite against your deliverable and runs it, along with the suites from every previous module. You never write the grading tests — you just have to pass them. See `OVERVIEW.md` → Verification and Grading.

See `OVERVIEW.md` for the full progression and detour table.

## Tracking Your Progress

On your personal branch, create:
- `SESSION_STATE.md` — track your current module and key learnings
- `module_*/QA_LOG.md` — log questions, answers, and checkpoint status per module

Start each `QA_LOG.md` from this template:

```markdown
# Module N — QA Log

## Readiness Answers
<!-- your answers to the Before You Start questions -->

## Questions & Answers
<!-- running log: instructor questions, your answers, corrections -->

## Grading
- [ ] Module test suite passing
- [ ] All previous modules' suites passing

## Checkpoint
- [ ] Checkpoint questions passed
```

Keep course material improvements on `main`. Keep your personal progress on your branch.

## Initial Prompt

Use this prompt with Claude Code to begin a tutoring session:

```
Hi Claude, teach me this course. Read CLAUDE.md and OVERVIEW.md first, then start
me at Module 0 (or wherever SESSION_STATE.md says I left off). Follow the teaching
style and grading process they describe: I write all the code, you guide, critique,
and write the grading tests. Be tough and critical in evaluating my understanding —
do not be my friend, and do not just give me answers. Ask questions to verify I am
learning, and ask for clarification when needed.
```
