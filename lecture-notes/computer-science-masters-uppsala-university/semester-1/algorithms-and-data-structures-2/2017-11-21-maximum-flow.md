# Lecture: Maximum Flow

Readings: Chapter 26, except Sections *26.4 and *26.5

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/07NetworkFlowI.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/07NetworkFlowI.pdf)

### Maximum Flow
- a flow network is a tuple ``G = (V, E, s, t, c)``
  - digraph ``(V, E)`` with source ``s ∈ V`` and sink ``t ∈ V``
  - non-negative capacity ``c(e)`` for each ``e ∈ E``
- an ``st``-cut (cut) is a partition ``(A, B)`` of the vertices with ``s ∈ A`` and ``t ∈ B``
  - its capacity is the sum of the capacities of the edges from ``A`` to ``B``
  - min-cut problem: find a cut of minimum capacity
- an ``st``-flow (flow) ``f`` is a function that satisfies
  - for each ``e ∈ E``: ``0 <= f(e) <= c(e)`` (capacity)
  - for each ``v ∈ V – {s, t}``, the incoming flow is equal to the outgoing flow (flow conservation)
  - the value of a flow ``f`` is equal to the flow out of a vertex ``-`` incoming flow into the vertex
  - max-flow problem: find a flow of maximum value

### Ford–Fulkerson algorithm
- greedy algorithm
  - start with ``f(e) = 0`` for each edge ``e ∈ E``
  - find an ``s↝t`` path ``P`` where each edge has ``f(e) < c(e)``
  - augment flow along path ``P``
  - repeat until you get stuck
  - why does the greedy algorithm fail?
    - once greedy algorithm increases flow on an edge, it never decreases it (need some mechanism to 'undo' bad decision)
- residual network
  - original edge ``e = (u, v) ∈ E``
    - flow ``f(e)``
    - capacity ``c(e)``
  - reverse edge ``e^reverse = (v, u)``
    - 'undo' flow sent
  - residual capacity
    - ``c_f(e) = c(e) - f(e) if e ∈ E``
    - ``c_f(e) = f(e) if e^reverse ∈ E``
  - residual network ``G_f = (V, E_f , s, t, cf )``
    - ``E_f = {e : f(e) < c(e)} U {e^reverse : f(e) > 0}``
    - key property: ``f'`` is a flow in ``G_f`` iff ``f + f'`` is a flow in ``G``
- augmenting path
  - an augmenting path is a simple ``s↝t`` path in the residual network ``G_f``
  - the bottleneck capacity of an augmenting path ``P`` is the minimum residual capacity of any edge in ``P``
  - key property: let ``f`` be a flow and let ``P`` be an augmenting path in ``G_f``. Then, after calling ``AUGMENT``, the resulting ``f'`` is a flow and ``val(f') = val(f) + bottleneck(G_f, P)``

### Max-flow min-cut theorem
- relationship between flows and cuts
  - let ``f`` be any flow and let ``(A, B)`` be any cut. Then, the value of the flow ``f`` equals the net flow across the cut ``(A, B)``
  - let ``f`` be any flow and ``(A, B)`` be any cut. Then, ``val(f) ≤ cap(A, B)``
- certificate of optimality
  - let ``f`` be a flow and let ``(A, B)`` be any cut
  - if ``val(f) = cap(A, B)``, then ``f`` is a max flow and ``(A, B)`` is a min cut
- augmenting path theorem: a flow ``f`` is a max flow iff no augmenting paths
- max-flow min-cut theorem: value of a max ``flow = capacity`` of a min cut

### Capacity-scaling algorithm
- analysis of Ford–Fulkerson algorithm (when capacities are integral)
  - capacities are integers between 1 and ``C``
  - throughout the algorithm, the flows ``f(e)`` and the residual capacities ``c_f(e)`` are integers
  - theorem: the algorithm terminates in at most ``val(f*) ≤ nC`` iterations, where ``f*`` is a max flow
  - the running time of Ford–Fulkerson is ``O(mnC)``
    - if ``C = 1``, the running time of Ford–Fulkerson is ``O(m n)``
  - integrality theorem: then exists a max flow ``f*`` for which every flow ``f*(e)`` is an integer
  - is generic Ford–Fulkerson algorithm poly-time in input size?
    - no, if max capacity is ``C``, then algorithm can take ``≥ C`` iterations
- choosing good augmenting paths
  - some choices lead to exponential algorithms
  - clever choices lead to polynomial algorithms
  - if capacities are irrational, algorithm not guaranteed to terminate (or converge to correct answer)!
  - goal is to choose augmenting paths so that:
    - can find augmenting paths efficiently
    - few iterations
  - choose augmenting paths with:
    - max bottleneck capacity ('fattest')
    - sufficiently large bottleneck capacity
    - fewest edges
  - intuition
    - choose augmenting path with highest bottleneck capacity: it increases flow by max possible amount in given iteration
    - don't worry about finding exact highest bottleneck path
    - maintain scaling parameter ``Δ``
    - let ``G_f(Δ)`` be the part of the residual network consisting of only those arcs with capacity ``≥ Δ``

### Shortest augmenting paths
- which augmenting path to select?
  - the one with the fewest edges (can be found via BFS)
- throughout the algorithm, the length of a shortest augmenting
path never decreases
- after at most ``m`` shortest-path augmentations, the length of a shortest augmenting path strictly increases
- the shortest-augmenting-path algorithm runs in ``O(m^2n)`` time
- level graph
  - given a digraph ``G = (V, E)`` with source ``s``, its level graph is defined by
    - ``ℓ(v)`` = number of edges in shortest path from ``s`` to ``v``
    - ``L_G = (V, E_G)`` is the subgraph of ``G`` that contains only those edges ``(v, w) ∈ E`` with ``ℓ(w) = ℓ(v) + 1``
  - can compute level graph in ``O(m + n)`` time (run BFS; delete back and side edges)
  - ``P`` is a shortest ``s↝v`` path in ``G`` iff ``P`` is an ``s↝v`` path ``L_G``
