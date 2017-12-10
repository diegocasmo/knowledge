# Lecture: Minimum Spanning Trees

Readings: CLRS3 Chapter 23 (we only study Prim's algorithm)

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsII.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsII.pdf)

### Minimum Spanning Trees (MST)
- a ``path`` is a sequence of edges which connects a sequence of nodes
- a ``cycle`` is a path with no repeated nodes or edges other than the starting and ending nodes
- a ``cut`` is a partition of the nodes into two nonempty subsets ``S`` and ``V-S``
- the ``cutset`` of a cut ``S`` is the set of edges with exactly one endpoint in ``S``
- a cycle and a cutset intersect in an even number of edges
- spanning tree:
  - let ``H=(V,T)`` be a subgraph of an undirected graph ``G=(V,E)``
  - ``H`` is a spanning tree of ``G`` if ``H`` is both acyclic and connected
- spanning tree properties:
  - let ``H=(V,T)`` be a subgraph of an undirected graph ``G=(V,E)``
    - ``H`` is a spanning tree of ``G``
    - ``H`` is acyclic and connected
    - ``H`` is connected and has ``n – 1`` edges
    - ``H`` is acyclic and has ``n – 1`` edges
    - ``H`` is minimally connected: removal of any edge disconnects it
    - ``H`` is maximally acyclic: addition of any edge creates a cycle
    - ``H`` has a unique simple path between every pair of nodes
- minimum spanning tree (MST)
  - given a connected, undirected graph ``G=(V,E)`` with edge costs ``c_e``, a minimum spanning tree ``(V,T)`` is a spanning tree of ``G`` such that the sum of the edge costs in ``T`` is minimized
  - notice there are ``n^(n–2)`` spanning trees of complete graph on ``n`` vertices (computationally expensive by brute force)
- fundamental cycle
  - let ``H=(V,T)`` be a spanning tree of ``G=(V,E)``
  - adding any non-tree edge ``e ∈ E`` to ``T`` forms unique cycle ``C``
  - deleting any edge ``f ∈ C`` from ``T U { e }`` results in a spanning tree
- fundamental cutset
  - let ``H=(V,T)`` be a spanning tree of ``G=(V,E)``
  - deleting any tree edge ``f`` from ``T`` divides nodes of spanning tree into two connected components. Let ``D`` be cutset
  - adding any edge ``e ∈ D`` to ``T – { f }`` results in a spanning tree
- the greedy algorithm to solve the MST problem is based on the following idea
  - let ``G=(V,E)`` be a connected, undirected graph with a real-valued weight function ``w`` defined on ``E``. Let ``A`` be a subset of ``E`` that is included in some minimum spanning tree for ``G``, and let C=(Vc, Ec) be a connected component (tree) in the forest ``Ga=(V,A)``. If ``(u, v)`` is a light edge connecting ``C`` to some other component in ``Ga``, then ``(u, v)``is safe for ``A``
    - an edge is a light edge crossing a cut if its weight is the minimum of any edge crossing the cut
- Prim's algorithm general idea (more detailed in book)
  - initialize ``S`` = any node, ``T = ∅``
  - repeat ``n – 1`` times:
    - add to ``T`` a min-weight edge with one endpoint in ``S``
    - add new node to ``S``
