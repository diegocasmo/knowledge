# Lecture: Dynamic Programming

Readings: CLRS3 Chapter 15

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/06DynamicProgrammingI.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/06DynamicProgrammingI.pdf)

Instructions:
  - Slides 1-15 & 23-29 & 37 of ``06DynamicProgrammingI.pdf``; self-study slides 30-36

### Dynamic Programming
- break up a problem into a series of ``overlapping`` subproblems, and build up solutions to larger and larger subproblems
  - basically, caching away intermediate results in a table for later use
- similar to problems solved by divide-and-conquer approach, but since there are overlapping subproblems, the divide-and-conquer approach does more work than necessary, repeatedly solving the common subproblems. Dynamic programming solves each subproblem only once and saves the answer somewhere it can be looked up, such that it can be used when solving the larger problems
- characteristics
  - optimal substructure: an optimal solution to the problem contains within it optimal solutions to subproblems
  - overlapping subproblems: dynamic programming algorithms typically take advantage of overlapping subproblems by solving each subproblem once and then storing the solution somewhere that can be looked up when needed
- typical approaches
  - top-down (memoization)
    - write the procedure recursively, but save subproblem results somewhere such other other recursion calls can check if the subproblem has already been solved
  - bottom-up
    - sort subproblems by size
    - solve the smaller subproblems first and then use their results when solving the larger subproblems
  - both approaches usually yield algorithms with the same asymptotic running time

### Weighted Interval Scheduling
- the problem
  - given
    - job ``j`` starts at ``s_j``, finishes at ``f_j``, and has weight or value ``v_j``
    - two jobs are compatible if they don't overlap
  - goal: find maximum-weight subset of mutually exclusive compatible jobs
- earliest finish-time first
  - consider jobs in ascending order of finish time
  - add job to subset if compatible with previously chosen jobs
  - greedy algorithm is correct if all weights are ``1``
  - greedy algorithm fails for weighted version
  - introduce notation
    - label jobs by finishing time: ``f1 <= f2 <= ... <= fn``
  - definition: ``p(j)`` is the largest index ``i < j`` such that job ``i`` is compatible with ``j``
- dynamic programming: binary choice
  - ``OPT(j)``: value of optimal solution to the problem consisting of job requests ``1, 2, ..., j``
  - goal: ``OPT(n)``, value of optimal solution to the original problem
  - case 1: ``OPT(j)`` selects job ``j``
    - collect profit ``v_j``
    - can't use incompatible jobs ``{ p(j) + 1, p(j) + 2, ..., j - 1 }``
    - must include optimal solution to problem consisting of remaining compatible jobs ``1, 2, ..., p(j)``
  - case 2: ``OPT(j)`` does not select job ``j``
    - must include optimal solution to problem consisting of remaining jobs ``1, 2, ..., j - 1``
  - recurrence ``OPT(j)``
    - ``0`` if ``j = 0``
    - ``max { v_j + OPT(p(j)), OPT(j - 1) }`` otherwise
  - this algorithm runs in ``O(n log n)`` with either bottom-up of top-down approach (unless jobs are already sorted, in which case it takes ``O(n)``)

### Knapsack Problem
- the problem
  - given
    - ``n`` items and a "knapsack"
    - item ``i`` weighs ``w_i > 0`` and has value ``v_i > 0``
    - knapsack has weight capacity of ``W``
  - goal: pack knapsack so as to maximize total value
- greedy algorithms
  - greedy by value: repeatedly add item with maximum ``v_i``
  - greedy by weight: repeatedly add item with minimum ``w_i``
  - greedy by ratio: repeatedly add item with maximum ratio ``v_i/w_i``
  - observation: none of these greedy algorithms is optimal
- dynamic programming
  - ``OPT(i, w)`` is max-profit subset of items ``1, ..., i`` with weight limit ``w``
  - goal: ``OPT(n, W)``
  - case 1: ``OPT(i, w)`` does not select item ``i``
    - ``OPT(i, w)`` selects best of ``{ 1, 2, ..., i - 1 }`` using weight limit ``w``
  - case 2: ``OPT(i, w)`` selects item ``i``
    - collect value ``v_i``
    - new weight limit ``w - w_i``
    - ``OPT(i, w)`` selects best of ``{ 1, 2, ..., i - 1 }`` using this new weight limit
  - recurrence ``OPT(i, w)``
    - ``0`` if ``i = 0``
    - ``OPT(i - 1, w)`` if ``w_i > w``
    - ``max { OPT(i - 1, w), v_i + OPT(i - 1, w - w_i) }`` otherwise
- remarks
  - not polynomial in input size (pseudo-polynomial)
  - decision version of knapsack problem is NP-complete
