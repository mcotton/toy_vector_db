# Module 4: Filtering and Metadata

**Why this matters:** Real queries are "find similar vectors WHERE category='X'", not just "find similar vectors."

## Topics
1. Post-filtering: search first, filter results after
2. Pre-filtering: filter candidates, search within the subset
3. Over-fetching: request more than k results, filter, return top k
4. Metadata storage and inverted indexes

## Deliverable
Add metadata filtering to search: `search(query, k, filters={"category": "electronics"})`. Implement post-filtering first, then discuss why it's inadequate.

## Checkpoint Questions
- Why can post-filtering return fewer than k results?
- Why can pre-filtering miss true nearest neighbors?
- How do production systems handle this?
