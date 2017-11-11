# Lecture: Greedy Algorithms

Readings: Chapter 16, except Sections *16.4 and *16.5: you are urged to study the additional example in Section 16.3

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsI.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsI.pdf)

### Greedy Algorithms
- a greedy algorithm always makes the choice that looks the best at the moment
- in other words, it make a locally optimal choice in the hope that this choice will lead to a globally optimal solution
- greedy algorithms do not always lead to an optimal solution, but sometimes they do
- in general greedy algorithms are designed according to the following sequence:
  - cast the optimization problem as one in which we make a choice and are left with one subproblem to solve
  - prove that there is always an optimal solution to the original problem that makes the greedy choice (so that greedy choice is always safe)
  - demonstrate optimal substructure by showing that, having made the greedy choice, what remains is a subproblem with the property that if we combine an optimal solution to the subproblem with the greedy choice, we have made, we arrive at an optimal solution to the original problem
- there are two key ingredients in a greedy algorithm
  - greedy-choice property
    - we can assemble a globally optimal solution by making locally optimal (greedy) choices
  - optimal substructure
    - an optimal solution to the problem contains within it optimal solutions to subproblems
