# Lecture: Algorithm Analysis

Readings: CLRS3 Chapter 3

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/02AlgorithmAnalysis.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/02AlgorithmAnalysis.pdf)

Instructions:
  - ``02AlgorithmAnalysis.pdf``

### Computational Tractability
- asymptotic analysis is the study of how the running time of an algorithm increases with the size of the input in the limit
- for many nontrivial problems there's a brute force approach, but this is usually unacceptable in practice
- polynomial running time is the desired scaling property, which means that when the input size doubles, the algorithm should slow down by at most some constant factor ``C``
  - an algorithm is poly-time if it holds the following: there exist constants ``c > 0`` and ``d > 0`` such that, for every input of size ``n``, the running time of the algorithm is bounded above by ``cn^d`` primitive computational steps
- an algorithm that runs in polynomial time is said to be efficient
- type of analysis
  - worst-case: running time guarantee for any input size ``n``
    - draconian view, but hard to find effective alternative
  - probabilistic: expected running time of a randomized algorithm
  - amortized: worst-case running time for any sequence of ``n`` operations
  - average case: expected running time for a random input of size ``n``

### Asymptotic Order of Growth
- big-oh notation
  - upper bounds: ``f(n)`` is ``O(g(n))`` if there exist constants ``c > 0`` and ``n0 ≥ 0`` such that ``f(n) ≤ c*g(n)`` for all ``n ≥ n0``
  - domain: the domain of ``g(n)`` is typically the natural numbers ``{ 0, 1, 2, ... }``
  - non-negative functions: when using big-oh notation, we assume that the functions involved are asymptotically non-negative
  - insertion sort makes ``O(n^2)`` comparison to sort ``n`` elements
  - example:
``` bash
f(n) = 32n^2 + 17n + 1 # is f(n) <= c*g(n)?
32n^2 + 17n + 1 <= n * g(n) # let g(n) = n^2, c = 50, n0 = 1
32 + 17 + 1 <= 50
50 <= 50
```
- big-omega notation
  - lower bounds: ``f(n)`` is ``Ω(g(n))`` if there exist constants ``c > 0`` and ``n0 ≥ 0`` such that ``f(n) ≥ c*g(n)`` for all ``n ≥ n0``
  - any compare-based soring algorithm requires ``Ω(n log n)`` comparison in the worst case
  - example:
``` bash
f(n) = 32n^2 + 17n + 1 # is f(n) ≥ c*g(n)?
32n^2 + 17n + 1 >= c * g(n) # let g(n) = n^2, c = 32, n0 = 1
32 + 17 + 1 >= 32
50 >= 32
```
- big-theta notation
  - tight bounds:  ``f(n)`` is ``Θ(g(n))`` if there exist constants ``c1 > 0``, ``c2 > 0``, and ``n0 ≥ 0`` such that ``c1*g(n) ≤ f(n) ≤ c2*g(n)`` for all ``n ≥ n0``
  - mergesort makes ``Θ(n log n)`` comparison to sort ``n`` elements
  - example:
``` bash
f(n) = 32n^2 + 17n + 1 # is c1*g(n) ≤ f(n) ≤ c2*g(n)?
c1*g(n) ≤ f(n) = 32n^2 + 17n + 1 ≤ c2*g(n) # let g(n) = n^2, c1 = 32, c2 = 50, n0 = 1
32 <= 50 <= 50
```

### Survey of Common Running Times (see detailed examples in slides)
- sub-linear time: ``O(log n)`` (also written as ``o(n)`` - little-oh)
  - running time grows in proportion to the logarithm of the input size
  - examples: binary search, binary trees
- linear time: ``O(n)``
  - running time is proportional to the input size
  - examples: merging two sorted lists, finding the minimal value in an array sorted in ascending order
- linearithmic time: ``O(n log n)``
  - usually arises in divide-and-conquer algorithms
  - examples: mergesort, heapsort
- quadratic time: ``O(n^2)``
  - enumerate all pair elements
  - format usually consists of a double ``for`` loop
  - examples: given a list of ``n`` points in the plane ``(x1, y1), …, (xn, yn)`` find the pair that is closest to each other
- cubic time: ``O(n^3)``
  - enumerate all triples of elements
  - format usually consists of a triple ``for`` loop
  - examples: given ``n`` sets ``S1, …, Sn`` each of which is a subset of ``1, 2, …, n``, is there some pair of these which are disjoint?
- polynomial time: ``O(n^k)``
  - running time is upper bounded by a polynomial expression in the size of the input for the algorithm
  - ``k`` is a given constant
  - examples: given a graph, are there ``k`` nodes such that no two are joined by an edge?
- exponential time: ``O(k^n)``
  - number of operations are proportional to the exponent of the size of input
  - examples: given a graph, what is maximum cardinality of an independent set?
