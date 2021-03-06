# Lecture: Dijkstra's Algorithm

Readings: CLRS3 Chapter 24: pages 643-650 and Section 24.3 (we only study Dijkstra's algorithm)

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsII.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsII.pdf)

Instructions
- Slides 1-12 of ``04GreedyAlgorithmsII.pdf``; ``04DemoDijkstra.pdf``

### Dijkstra's Algorithm
- single-pair shortest path problem
  - given a digraph ``G = (V, E)``, edge lengths ``e ≥ 0``, source ``s ∈ V``, and destination ``t ∈ V``, find a shortest directed path from ``s`` to ``t``
- single-source shortest paths problem
  - given a digraph ``G = (V, E)``, edge lengths ``e ≥ 0``, source ``s ∈ V``, find a shortest directed path from ``s`` to every node
- notice we assume the weights of the edges is ``>=0`` as we'll be studying Dijkstra's algorithm
- shortest path algorithms typically rely on optimal substructure to make a greedy choice
  - if the shortest path from ``s`` to ``t`` is ``s->u->v->t``, then the shortest path form ``s`` to ``v`` is ``s->u->v``
- in general, we want to know not only the weight of the shortest path, but also the vertices on it, thus we use the following representation:
  - ``v.π`` is a predecessor of ``v`` (which is ``NIL`` at the start of the algorithm)
  - ``v.d`` is an upper bound on the weight of a shortest path from source ``s`` to ``v``
- in general, single source algorithms start by initializing ``v ∈ V`` to  ``v.π=NIL`` and ``v.d=+Inf``. Then ``s.d`` is set to ``0``
- the process of relaxing an edge ``(u,v)`` consists of whether we can improve the shortest path to ``v`` found so far by going through ``u``. If so, we update both ``v.π`` and ``v.d``
- Dijkstra's algorithm
  - maintain a set of explored nodes ``S`` for which the algorithm has determined ``d[u]`` = length of a shortest ``s↝u`` path
    - initialize ``S ← { s }``, ``d[s] ← 0``
    - repeatedly choose unexplored node ``v ∉ S`` which minimizes ``d`` (i.e update all frontier neighbors ``π`` and ``d`` if necessary, and choose the one with the least ``d``)
  - invariant. For each node ``u ∈ S`` : ``d[u]`` = length of a shortest ``s↝u`` path
  - the original implementation of Dijkstra's algorithm does not mention the use of a min priority queue and it re-computes the weight ``d`` of each ``v ∉ S`` in the frontier
  - instead, use a min-oriented priority queue ordered by ``d``. Only needs to update ``d`` and ``π`` leaving the recently added ``u ∈ S``
- Dijkstra's algorithm time complexity is dependent on the implementation of the min priority queue
  - array implementation optimal for dense graphs (``Θ(n^2)`` edges)
  - binary heap much faster for sparse graphs (``Θ(n)`` edges)
  - 4-way heap worth the trouble in performance-critical solutions
  - Fibonacci heap best in theory, but probably not worth implementing
