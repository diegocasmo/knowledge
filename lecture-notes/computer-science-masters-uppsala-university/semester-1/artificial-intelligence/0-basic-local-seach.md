# Lecture: Basic & Local Search

Readings: Ch. 3, Russell, Stuart Jonathan; Norvig, Peter Artificial Intelligence: A Modern Approach 3

### Search
- Concepts
  - state space
    - the set of all states reachable from the initial state by any sequence of actions
    - This is defined by the initial state, actions, and the transition model
  - Search tree
    - a search tree is form by the possible action sequences starting at the initial state, with the initial state at the root of it. The branches are actions and the nodes correspond to states in the state space of the problem
- Graph search
  - consists of a set of visited nodes, initially empty
  - a set of frontier nodes (whose existence we know of but whose neighbors we are ignorant of). Initially contains only the origin
  - at each step:
    - select node from frontier and expand it (perform all possible transitions on the node to find its neighbors)
    - if the goal is one of the neighbors, terminate the search; else
      - add node to the visited set
      - add those neighbors that are not already in the visited set or frontier to the frontier
- Tree search
  - consists of a set of frontier nodes, whose existence we know of but whose neighbors we are ignorant of. Initially contains only the origin
  - at each step:
    - select a node from frontier and expand it (perform all possible transitions on the node to find its neighbors)
    - if goal is one of the neighbors, terminate search, else:
      - remove the node from the frontier
      - add those neighbors that are not already in the frontier set to the frontier

### Uninformed Search Strategies
- a directed graph, where nodes are states, and edges transitions
- algorithms
  - breadth first search (BFS):
    - frontier is a FIFO queue
    - expand all nodes in a layer before progressing to the next layer
    - goal test is applied to each node when when it's generated, rather than when it's selected for expansion (goal test is applied to neighbor)
    - requires a lot of memory ``O(b^d)``
    - can take a lot of time to execute ``O(b^d)``
    - optimal: Yes
    - complete: Yes
  - depth first search (DFS):
    - frontier is a LIFO stack
    - expand the deepest node in the current frontier of the search tree
    - the frontier of DFS ``O(b*d)`` is much less than the frontier of BFS ``O(b^d)``
    - optimal: no
    - complete:
      - yes if using graph search (because it avoids repeated states)
      - no if using tree search
        - it won't find a solution if the search space is infinite
        - or if the search space has directed loops
  - iterative deepening:
    - problem: the advantages of DFS are only available in acyclic (loopless) finite search spaces
    - solution: run DFS algorithm repeatedly with a tuning parameter ``t=1,2,3...``
    - at each iteration, we assume there are no transitions from the nodes at ``t``
    - optimal: yes
    - complete: yes
    - in general, iterative deepening is the preferred uninformed search method when the search space is large and the depth of the solution is not known
  - bidirectional Search:
    - the idea behind bidirectional search is to run two simultaneous searches - one forward from the initial state and the other backward from the goal - hoping that the two searches meet in the middle
    - at least one of the frontiers must be kept in memory, and this space requirement is one of the most significant weakness of this algorithm
    - optimal: yes
    - complete: yes

### Local Search
  - a directed graph and a fitness function (takes a state as an argument and returns a value)
  - algorithms
    - best first search
      - frontier is ordered by fitness. At each step of the search process, we expand the fittest currently known node
        - uses a priority FIFO queue ordered by ``f``
          - which typically include a heuristic function ``h(n)``
    - local search
      - we have
        - the current node, initially the origin
        - a transition function, ``f(n, N) = m``, which takes the current node, ``n``, and a set of nodes, ``N``, and returns a node ``m`` from that set
        - a termination function, ``g(n, m T)``, that takes the current node, ``n``, another node, ``m``, and a possible tuning parameters, ``T``, as arguments and returns a boolean value that decides whether the algorithm terminates
      - at each step we:
        - expand the current node, ``n`` (find all possible transitions on it to find its neighbors, which are ``N``)
        - find the potential transition ``m=f(n,M)`` and see if we should transition to ``m`` or terminate and return the current node as the output of the algorithm by checking ``g(n, m, T)``
    - greedy hill climb
        - at each step in the search process, transition to the fittest neighbor node if it is fitter than the current node. Otherwise we terminate the search, selecting the current node as the solution
        - local maxima (optima): a pick that is higher than each of its neighboring states, but lower than the global maximum
    - simulated annealing
      - this is an extension of greedy hill climb which introduces the idea of sometimes going down
      - given current node ``n``, and neighbor node ``m``, the algorithm will decide whether to transition from ``n`` to ``m`` probabilistically
        - usually, this transition happens with probability 1 if ``m`` is fitter than ``n``
        - otherwise, the transition depends on the relative fitness of ``n`` and ``m``, and a tuning parameter, called the temperature ``t``
        - ``t`` gradually approaches 0 according to a schedule, and the chances of choosing a less fit state approaches 0 as ``t`` approaches 0
      - attractive local optima: the nature of SA means that it is unlikely to go on long expeditions downhill, especially as the temperature cools down (it’s unable to break free of an attractive local optima)
  - local beam search
    - select ``n`` initial nodes, each of them termed a 'particle'
    - at each step
      - create a set ``S`` of the nodes consisting of the nodes the particles are currently at and the neighbors of these nodes
      - the particles transition to the ``n`` fittest nodes in ``S``
    - search terminates when no transitions are made
    - notice we can transition to a neighbor that’s not ours (because another particle found multiple fitter nodes)
    - this algorithm is a simple example of swarm intelligence
    - a problem of this algorithms is that particles usually end up being clustered together, which limits the effectiveness of this strategy (the idea was to get a greater awareness of the state space than GHC, but this behavior limits it)
  - stochastic local beam search
    - select ``n`` initial nodes, each of them termed a 'particle'
    - at each step
      - create a set ``S`` of the nodes consisting of the nodes the particles are currently at and the neighbors of these nodes
      - the particles transition to ``n`` nodes in ``S`` drawn at random but weighted by their relative fitness
    - search terminates when no neighbor node is better than any of the current nodes
    - this algorithm above the clustering behavior of local beam search (LBS) and is generally preferable
