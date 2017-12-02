# Lecture: Heuristic & Competitive Search

Readings: Ch. 4 & 5, Russell, Stuart Jonathan; Norvig, Peter Artificial Intelligence: A Modern Approach 3

### Finding The Shortest Path
- a directed graph, edges have cost values
- dynamic programming for path finding
  - assign the origin the value of ``0``
  - proceed iteratively through each layer ``L`` of the state space
  - treat the origin as the ``0`` layer and begin at layer ``1``
  - backtrack from goal to the origin to find the optimal path
- A* search
  - best first-search where ``f(n) = g(n) + h(n)``
  - an admissible (optimistic) heuristic is one that never overestimates the cost to reach a goal
  - ``f(n)``: estimated shortest path from start to goal via ``n``
  - ``g(n)``: length of shortest path to ``n`` found so far
  - ``h(n)``: estimated distance from ``n`` to goal
  - ``f*(n)``: actual shortest distance from start to goal via ``n``
  - ``g*(n)``: length of actual shortest path to ``n``
  - ``h*(n)``: actual shortest path from ``n`` to goal
  - ``h``is optimistic if ``h(n)<=h*(n)``
  - ``h`` is monotonic
    - triangle inequality rule: each side of a triangle cannot be longer than the sum of the two other sides
  - A* search guarantees that when we have visited the goal, we have found the shortest path
    - A* using tree-search requires optimism: the tree-search version is optimal if ``h(n)`` is admissible
    - A* using graph-search and therefore not revisiting visited nodes requires monotonicity (which implies optimism): the graph-search version is optimal if ``h(n)`` is consistent
  - selecting the heuristic
    - a good estimate is one which is close to the actual value
    - the worst optimistic and monotonic heuristic is ``h(n)=0``. This makes A* just look at the cost of getting to nodes and is equivalent to best-first search
    - a good heuristic is usually domain dependent, but if all edges have cost ``>= 1``, the minimum number of steps between a node and the goal is optimistic and monotonic

### Competitive Search
- minimax
  - performs a recursive computation of the best moves for each player given successor states
  - search space is too large to perform the complete algorithm
    - so search is normally performed to some depth and a heuristic analysis of the state of the game is given at that depth
  - heuristic
    - positive if state believed is good for MAX
    - negative if state believed is good for MIN
- alpha-beta Pruning
  - optimization of the minimax algorithm which prunes branches of the tree which the other opponent won’t let the other player take
  - Increases the search-able depth by orders of magnitude
  - Selecting initial moves that return a small [alpha, beta] interval of possible moves can help enormously since these will rule out lost of following branches
- issues
  - suffer from horizon effect
    - a bad thing will surely happen, but you can postpone it beyond the search depth (trying to save a piece that will surely die, and sacrificing others because of that)
    - these approaches do not permit the algorithms to "wager" that an opponent will make a mistake
