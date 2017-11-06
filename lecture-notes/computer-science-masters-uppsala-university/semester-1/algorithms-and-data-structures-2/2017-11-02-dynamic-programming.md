# Lecture: Dynamic Programming

Readings: CLRS3 Chapter 15

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/06DynamicProgrammingI.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/06DynamicProgrammingI.pdf)

### Dynamic Programming
- general idea: break up a problem into a series of ``overlapping`` subproblems, and build up solutions to larger and larger subproblems
  - basically, caching away intermediate results in a table for later use
- similar to problems solved by divide-and-conquer approach, but since there are overlapping subproblems, the divide-and-conquer approach does more work than necessary, repeatedly solving the common subproblems. Dynamic programming solves each subproblem only once and saves the answer somewhere it can be looked up, such that it can be used when solving the larger problems
- characteristics
  - optimal substructure
    - an optimal solution to the problem contains within it optimal solutions to subproblems
  - overlapping subproblems
    - dynamic programming algorithms typically take advantage of overlapping subproblems by solving each subproblem once and then storing the solution somewhere that  can be looked up when needed
- typical approaches
  - top-down (memoization)
    - write the procedure recursively, but save subproblem results somewhere such other other recursion calls can check if the subproblem has already been solved
  - bottom-up
    - sort subproblems by size
    - solve the smaller subproblems first and then use their results when solving the larger subproblems
  - both approaches usually yield algorithms with the same asymptotic running time
