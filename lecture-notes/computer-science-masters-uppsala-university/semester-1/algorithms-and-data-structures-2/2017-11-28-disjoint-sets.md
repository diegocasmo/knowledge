# Lecture: Disjoint Sets

Readings: Chapter 21, except Sections 21.2 and 21.4

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/UnionFind.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/UnionFind.pdf)

### Disjoint-sets Data Type
- original motivation: compiling ``EQUIVALENCE``, ``DIMENSION``, and ``COMMON`` statements in Fortran
- support three operations on a collection of disjoint sets
  - ``MAKE-SET(x)``: create a new set containing only element ``x``
  - ``FIND(x)``: return a canonical element in the set containing ``x``
  - ``UNION(x, y)``: replace the sets containing ``x`` and ``y`` with their union
- performance parameters
  - ``m`` = number of calls to ``MAKE-SET``, ``FIND``, and ``UNION``
  - ``n`` = number of elements = number of calls to ``MAKE-SET``
- dynamic connectivity
  - given an initially empty graph ``G``, support three operations:
    - ``ADD-NODE(u)``: add node ``u``
    - ``ADD-EDGE(u, v)``: add an edge between nodes ``u`` and ``v``
    - ``IS-CONNECTED(u, v)``: is there a path between ``u`` and ``v``?
- parent-link representation
  - represent each set as a tree of elements
  - each element has an explicit parent pointer in the tree
  - the root serves as the canonical element (and points to itself)
  - ``FIND(x)``: find the root of the tree containing ``x``
  - ``UNION(x, y)``: merge trees containing ``x`` and ``y``
- array representation
  - represent each set as a tree of elements
  - allocate an array ``parent[]`` of length ``n`` (must know number of elements ``n`` a priori)
  - ``parent[i] = j`` means parent of element ``i`` is element ``j``

### Naïve Linking (pseudo-code in slides)
- link root of first tree to root of second tree
- using naïve linking, a ``UNION`` or ``FIND`` operation can take ``Θ(n)`` time in the worst case, where ``n`` is the number of elements (in the worst case, ``FIND`` takes time proportional to the height of the tree)

### Link-By-Size (pseudo-code in slides)
- maintain a tree size (number of nodes) for each root node
- link root of smaller tree to root of larger tree (breaking ties arbitrarily)
- using link-by-size, for every root node ``r`` : ``size[r] ≥ 2^height(r)``
- using link-by-size, any ``UNION`` or ``FIND`` operation takes ``O(log n)`` time in the worst case, where ``n`` is the number of elements
  - the running time of each operation is bounded by the tree height
  - by the previous property, the height is ``≤ ⎣lg n⎦`` (base 2)
  - the ``UNION`` operation takes ``O(1)`` time except for its two calls to ``FIND``

### Link-By-Rank (pseudo-code in slides)
- maintain an integer rank for each node, initially ``0``
- link root of smaller rank to root of larger rank; if tie, increase rank of new root by ``1``
- properties
  - 1) if ``x`` is not a root node, then ``rank[x] < rank[parent[x]]``
    - a node of rank ``k`` is created only by linking two roots of rank ``k – 1``
  - 2) if ``x`` is not a root node, then ``rank[x]`` will never change again
    - rank changes only for roots; a non-root never becomes a root
  - 3) if ``parent[x]`` changes, then ``rank[parent[x]]`` strictly increases
    - the parent can change only for a root, so before linking ``parent[x] = x``
    - after ``x`` is linked-by-rank to new root ``r`` we have ``rank[r] > rank[x]``
  - 4) any root node of rank ``k`` has ``≥ 2^k`` nodes in its tree
  - 5) the highest rank of a node is ``≤ ⎣lg n⎦``
  - 6) for any integer ``k ≥ 0``, there are ``≤ n / 2^k`` nodes with rank ``k``
- using link-by-rank, any ``UNION`` or ``FIND`` operation takes ``O(log n)`` time in the worst case, where ``n`` is the number of elements

### Path Compression (pseudo-code in slides)
- when finding the root ``r`` of the tree containing ``x``, change the parent pointer of all nodes along the path to point directly to ``r``
- notice the ``FIND`` implementation changes the tree structure
- path compression does not change the rank of a node; so ``height(x) ≤ rank[x]`` but they are not necessarily equal
- naïve linking
  - path compression with naïve linking can require ``Ω(n)`` time to perform a single ``UNION`` or ``FIND`` operation, where ``n`` is the number of elements
  - starting from an empty data structure, path compression with naïve linking performs any intermixed sequence of ``m ≥ n`` ``MAKE-SET``, ``UNION``, and ``FIND`` operations on a set of ``n`` elements in ``O(m log n)`` time
- link-by-rank
  - the tree roots, node ranks, and elements within a tree are the same with or without path compression
  - starting from an empty data structure, link-by-rank with path compression performs any intermixed sequence of ``m ≥ n`` ``MAKE-SET``, ``UNION``, and ``FIND`` operations on a set of n elements in ``O(m log*n)`` time
  - property 2, 4–6 hold for link-by-rank with path compression (need to revise properties 1 and 3)
    - 1) If ``x`` is not a root node, then ``rank[x] < rank[parent[x]]``
      - path compression doesn't change any ranks, but it can change parents. If ``parent[x]`` doesn't change during a path compression, the inequality continues to hold; if ``parent[x]`` changes, then ``rank[parent[x]]`` strictly increases
    - 3) if parent[x] changes, then rank[parent[x]] strictly increases
      - path compression can make ``x`` point to only an ancestor of ``parent[x]``
  - the iterated logarithm function is defined by:
    - ``log* n =``
      - ``0`` if ``n`` <= ``1``
      - ``1 + lg* (lgn)`` otherwise
    - we have ``lg* n ≤ 5`` unless n exceeds the # atoms in the universe
  - starting from an empty data structure, link-by-rank with path compression performs any intermixed sequence of ``m ≥ n`` ``MAKE-SET``, ``UNION``, and ``FIND`` operations on a set of n elements in ``O(m log*n)`` time
