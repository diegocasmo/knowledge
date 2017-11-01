# Lecture: Divide-and-conquer

Readings: CLRS3 Chapter 4 (except 4.6)

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/05DivideAndConquerI.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/05DivideAndConquerI.pdf)

### Divide-and-conquer
- The gist
  - divide up problem into several subproblems (of the same kind)
  - solve each problem recursively
  - combine solutions to subproblems into overall solution
- Most common usage
  - divide problem of size ``n`` into two subproblems of size ``n/2`` in linear time
  - solve two subproblems recursively
  - combine two subproblems into overall solution in linear time
  - result: Θ(n log n)

### Mergesort
- Problem: given a list of ``n`` elements, re-arrange them in ascending order
- Algorithm
  - recursively sort left half
  - recursively sort right half
  - merge two halves to make a sorted whole
- Merge sort is ``O(n log n)`` (see detailed proofs in slides)
- When creating a proof, a recursion tree can be a powerful tool to provide intuition to later on work on a formal proof
