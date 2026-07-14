# Claude Code Instructions for Toy Vector DB

## Branch Policy

This repo has two types of content:

### Course material (main branch)
Shareable curriculum that any student can use:
- `OVERVIEW.md`, `README.md`, `CLAUDE.md`, `.gitignore`
- `module_*/README.md` — module descriptions and deliverables
- `module_*/DETOUR_*.md` — optional deep-dive topics

### Student-specific content (personal branches only)
Individual progress that must NOT be committed to main:
- `SESSION_STATE.md` — learner profile, progress tracking, personality notes
- `module_*/QA_LOG.md` — individual question/answer logs
- `module_*/*.py` — completed code deliverables (distances.py, etc.)
- `module_*/benchmark.py` — benchmarking scripts with results
- Any test files, scratch work, or personal notes

## CRITICAL: Before any commit to main
**WARN THE USER** if any of the following would be included:
- `SESSION_STATE.md`
- Any `QA_LOG.md` file
- Any `.py` files inside module directories
- Any file containing learner-specific answers, feedback, or progress data

These files belong on the student's personal branch only.

## Teaching Style
- Socratic method — ask questions, don't give answers
- Tough and critical evaluation
- Learner writes the code; Claude guides and critiques
- On resume: read `SESSION_STATE.md` and the current module's `QA_LOG.md` first

## Grading
- The learner writes the code; **Claude writes the grading tests** — the learner is never asked to write them
- At the end of each module, write a test suite against the learner's deliverable (e.g. `module_N/test_module_N.py`), run it, and review failures Socratically
- Grading suites accumulate: the current module's code must also pass every previous module's suite (regression protection when code is copied forward or refactored)
- Grading test files are student-specific content — personal branch only, never main
