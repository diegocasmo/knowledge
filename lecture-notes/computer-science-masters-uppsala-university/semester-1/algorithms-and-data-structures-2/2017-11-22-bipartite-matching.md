# Lecture: Bipartite Matching

Readings: CLRS3 Chapter 26, except Sections 26.4 and 26.5

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/07NetworkFlowII.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/07NetworkFlowII.pdf)

Instructions
- slides 1-11 & 15-17 & 20-37 (without the proofs) of ``07NetworkFlowII.pdf`` and self-study its slides 38-73 if you are interested

### Bipartite Matching
- given an undirected graph ``G = (V, E)`` a subset of edges ``M ⊆ E`` is a matching if each node appears in at most one edge in ``M``.
  - max matching: given a graph, find a max-cardinality matching
- a graph ``G`` is bipartite if the nodes can be partitioned into two subsets ``L`` and ``R`` such that every edge connects a node in ``L`` to one in ``R``
  - bipartite matching: given a bipartite graph ``G = (L ∪ R, E)``, find a max-cardinality matching
  - max-flow formulation
    - create digraph ``G' = (L ∪ R ∪ {s, t}, E' )``
    - direct all edges from ``L`` to ``R``, and assign infinite (or unit) capacity
    - add source ``s``, and unit-capacity edges from ``s`` to each node in ``L``
    - add sink ``t``, and unit-capacity edges from each node in ``R`` to ``t``
    - theorem: max cardinality of a matching in ``G`` = value of max flow in ``G'``
  - given a graph ``G = (V, E)`` a subset of edges ``M ⊆ E ``is a perfect matching if each node appears in exactly one edge in ``M``

### Disjoint Paths
- edge-disjoint paths
  - two paths are edge-disjoint if they have no edge in common
  - disjoint path problem: given a digraph ``G = (V, E)`` and two nodes ``s`` and ``t``, find the max number of edge-disjoint ``s↝t`` paths
  - max-flow formulation
    - assign unit capacity to every edge
    - max number edge-disjoint ``s↝t`` paths equals value of max flow
- network connectivity
  - a set of edges ``F ⊆ E`` disconnects ``t`` from ``s`` if every ``s↝t`` path uses at least one edge in ``F``
  - given a digraph ``G = (V, E)`` and two nodes ``s`` and ``t``, find min number of edges whose removal disconnects ``t`` from ``s``
  - theorem: the max number of edge-disjoint ``s↝t`` paths equals the min number of edges whose removal disconnects ``t`` from ``s``
- edge-disjoint paths in undirected graphs
  - two paths are edge-disjoint if they have no edge in common
  - given a graph ``G = (V, E)`` and two nodes ``s`` and ``t``, find the max number of edge-disjoint ``s–t`` paths
  - max-flow formulation
    - replace each edge with two anti-parallel edges and assign unit capacity to every edge (two paths ``P1`` and ``P2`` may be edge-disjoint in the digraph but not edge-disjoint in the undirected graph)
    - in any flow network, there exists a maximum flow ``f`` in which for each pair of anti-parallel edges ``e`` and ``e'``, either ``f(e) = 0`` or ``f(e') = 0`` or both. Moreover, integrality theorem still holds
    - theorem: max number edge-disjoint ``s↝t`` paths equals value of max flow
- Menger's theorems
  - given an undirected graph with two nodes ``s`` and ``t``, the max number of edge-disjoint ``s–t`` paths equals the min number of edges whose removal disconnects ``s`` and ``t``
  - given an undirected graph with two nonadjacent nodes ``s`` and ``t``, the max number of internally node-disjoint ``s–t`` paths equals the min number of internal nodes whose removal disconnects ``s`` and ``t``
  - given a directed graph with two nonadjacent nodes ``s`` and ``t``, the max number of internally node-disjoint ``s↝t`` paths equals the min number of internal nodes whose removal disconnects ``t`` from ``s``

### Extensions to max flow
- given a digraph ``G = (V, E)`` with non-negative edge capacities ``c(e)`` and multiple source nodes and multiple sink nodes, find maximum flow that can be sent from the source nodes to the sink nodes
  - add a new source node ``s`` and sink node ``t``
  - for each original source node ``s_i`` add edge ``(s, s_i)`` with capacity ``∞``
  - for each original sink node ``t_j``, add edge ``(t_j, t)`` with capacity ``∞``
