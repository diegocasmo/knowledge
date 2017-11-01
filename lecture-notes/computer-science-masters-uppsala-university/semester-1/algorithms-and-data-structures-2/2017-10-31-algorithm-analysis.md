# Lecture: Algorithm Analysis

Readings: CLRS3 Chapter 3

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/02AlgorithmAnalysis.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/02AlgorithmAnalysis.pdf)

### Asymptotic Order of Growth
- Asymptotic analysis is the study of how the running time of an algorithm increases with the size of the input in the limit
- For many nontrivial problems there's a brute force approach, but this is usually unacceptable in practice
- Polynomial running time is the desired scaling property, which means that when the input size doubles, the algorithm should slow down by at most some constant factor ``C``
  - an algorithm is poly-time if it holds the following: there exist constants ``c > 0`` and ``d > 0`` such that, for every input of size ``n``, the running time of the algorithm is bounded above by ``cn^d`` primitive computational steps
- Worst case analysis refers to the idea of the analysis of an algorithm running time guarantee for any input of size ``n``
  - draconian view, but hard to find effective alternative
- Big-Oh notation
  - upper bounds: ``T(n)`` is ``O(f(n))`` if there exist constants ``c > 0`` and ``n0 ≥ 0`` such that ``T(n) ≤ c · f(n)`` for all ``n ≥ n0``
  - domain: the domain of ``f(n)`` is typically the natural numbers ``{ 0, 1, 2, … }``
  - non-negative functions: when using big-Oh notation, we assume that the functions involved are (asymptotically) non-negative
- Big-Omega notation
  - lower bounds: ``T(n)`` is ``Ω(f(n))`` if there exist constants ``c > 0`` and ``n0 ≥ 0`` such that ``T(n) ≥ c · f(n)`` for all ``n ≥ n0``
- Big-Theta notation
  - tight bounds:  ``T(n)`` is ``Θ(f(n))`` if there exist constants ``c1 > 0``, ``c2 > 0``, and ``n0 ≥ 0`` such that ``c1 · f(n) ≤ T(n) ≤ c2 · f(n)`` for all ``n ≥ n0``

### Survey of Common Running Times (see detailed examples in slides)
- Linear time: ``O(n)``
  - running time is proportional to the input size
  - examples: Merging two sorted lists, finding the minimal value in an array sorted in ascending order
- Logarithmic time (sub-linear time): ``O(log n)``
  - running time grows in proportion to the logarithm of the input size
  - examples: binary search, binary trees
- Linearithmic time: ``O(n log n)``
  - usually arises in divide-and-conquer algorithms
  - examples: Merge sort, heapsort
- Quadratic time: ``O(n^2)``
  - enumerate all pair elements
  - format usually consists of a double ``for`` loop
  - examples: given a list of ``n`` points in the plane ``(x1, y1), …, (xn, yn)`` find the pair that is closest to each other
- Cubic time: ``O(n^3)``
  - enumerate all triples of elements
  - format usually consists of a triple ``for`` loop
  - examples: given ``n`` sets ``S1, …, Sn`` each of which is a subset of ``1, 2, …, n``, is there some pair of these which are disjoint?
- Polynomial time: ``O(n^k)``
  - running time is upper bounded by a polynomial expression in the size of the input for the algorithm
  - ``k`` is a given constant
  - examples: given a graph, are there ``k`` nodes such that no two are joined by an edge?
- Exponential time: ``O(k^n)``
  - number of operations are proportional to the exponent of the size of input
  - examples: given a graph, what is maximum cardinality of an independent set?
