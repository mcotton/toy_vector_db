# Module 3A: Indexing — KD-Trees (Tree-Based Partitioning)

**Why first:** Intuitive spatial partitioning. Easy to visualize. Understanding why it fails in high dimensions is the key lesson.

## Topics
- Recursive binary space partitioning
- Choosing split dimensions and split values (median)
- Search: traversal + backtracking
- Why performance degrades exponentially with dimensions

## Deliverable
Implement a KD-Tree index. Run search on 100-dim and 768-dim data. Compare recall and speed against brute force.

## Checkpoint Questions
- At what dimensionality does your KD-Tree become slower than brute force? Why?
- How does the tree depth relate to the number of points?
- What's the worst-case query time?
