# The A* Search Algorithm

### Path Finding
Finding the optimal path between two states is a common problem in many areas of computer science such as planning, games, and scheduling. Given an initial position, goal, and a transition model, the objective is to find an optimal path towards the goal state. Optimal may be in terms of time (generally shortest) or resources (generally minimal use) or both.

The goal of this blog post is to explain the A* search algorithm, a well known and popular path finding algorithm which is guaranteed to find the goal state in the least number of steps (more on this later).

### What Is It?
The A* search algorithm is search procedure commonly used for path finding and graph traversal. It is an informed search strategy, which as opposed to an uninformed search strategy, uses problem specific knowledge beyond the definition of the problem itself. Similarly to other search techniques, the gist of the A* search algorithm is to continually expand nodes of a search space in a particular order until a goal state is found. It belongs to the family of best-first search algorithms, where nodes that are going to be expanded are stored in a priority queue ordered by ``f(n)``. The definition of ``f(n)``, that is, the way in which nodes that are going to be expanded next are ordered, is what primarily differentiates the A* search algorithm from other search algorithms.

- tree vs graph search
- heuristic driven search strategies
  - what is a heuristic?
  - what is a good heuristic?
- optimality and completeness
  - tree search requirements
  - graph search requirements
- space and time complexity
- pseudo code/actual code? (Wikipedia?)

### Conclusion
- summary of the concepts
- what have I learn
