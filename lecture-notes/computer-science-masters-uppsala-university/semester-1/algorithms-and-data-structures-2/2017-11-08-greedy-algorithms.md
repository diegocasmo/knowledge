# Lecture: Greedy Algorithms

Readings: CLRS3 Chapter 16, except Sections 16.4 and 16.5: you are urged to study the additional example in Section 16.3

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsI.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsI.pdf)

Instructions
- Slides 1-7 of ``04GreedyAlgorithmsI.pdf``; self-study slides 8-14
- ``04DemoEarliestFinishTimeFirst.pdf`` (corresponding to Section 16.1), if not also slides 15-22 & ``04DemoEarliestStartTimeFirst.pdf``

### Greedy Algorithms
- a greedy algorithm always makes the choice that looks the best at the moment
- in other words, it makes a locally optimal choice in the hope that this choice will lead to a globally optimal solution
- greedy algorithms do not always lead to an optimal solution
- in general greedy algorithms are designed according to the following sequence:
  - cast the optimization problem as one in which we make a choice and are left with one subproblem to solve
  - prove there is always an optimal solution to the original problem that makes the greedy choice (so that greedy choice is always safe)
  - demonstrate optimal substructure by showing that, having made the greedy choice, what remains is a subproblem with the property that if we combine an optimal solution to the subproblem with the greedy choice we have made, we arrive at an optimal solution to the original problem
- there are two key ingredients in a greedy algorithm
  - greedy-choice property
    - we can assemble a globally optimal solution by making locally optimal (greedy) choices
  - optimal substructure
    - an optimal solution to the problem contains within it optimal solutions to subproblems

### Coin Changing
- given a currency denominations: ``1, 5, 10, 25, 100`` devise a method to pay amount to customer using fewest coins
- cashier's algorithm: at each iteration, add coin of the largest value that does not take us past the amount to be paid
- the cashier's algorithm is optimal for U.S. coins, but it is not optimal for all set of denominations
  - in some scenarios it might not even lead to a feasible solution

### Interval Scheduling
- job ``j`` starts at ``s_j`` and finishes at ``f_j``
- two jobs compatible if they don't overlap
- goal: find maximum subset of mutually compatible jobs
- consider jobs in ascending order of ``f_j`` (earliest finish time first)
- the earliest-finish-time-first algorithm is optimal (notice the greedy algorithm does not produce an optimal solution to the weighted version of this problem - see dynamic programming algorithm instead)
