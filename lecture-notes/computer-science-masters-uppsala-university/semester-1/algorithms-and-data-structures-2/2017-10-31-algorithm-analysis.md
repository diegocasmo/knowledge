# Lecture: Algorithm Analysis
Readings: CLRS3 Chapters 3

- Asymptotic analysis is the study of how the running time of an algorithm increases with the size of the input in the limit
- For many nontrivial problems there's a brute force approach, but this is usually unacceptable in practice
- Polynomial running time is the desired scaling property, which means that when the input size doubles, the algorithm should slow down by at most some constant factor ``C``
  - an algorithm is poly-time if it holds the following: there exist constants ``c > 0`` and ``d > 0`` such that, for every input of size ``n``, the running time of the algorithm is bounded above by ``cn^d`` primitive computational steps
- Worst case analysis refers to the idea of the analysis of an algorithm running time guarantee for any input of size ``n``
  - Draconian view, but hard to find effective alternative
- Big-Oh notation
  - Upper bounds: ``T(n)`` is ``O(f(n))`` if there exist constants ``c > 0`` and ``n0 ≥ 0`` such that ``T(n) ≤ c · f(n)`` for all ``n ≥ n0``
  - domain: the domain of ``f(n)`` is typically the natural numbers ``{ 0, 1, 2, … }``
  - non-negative functions: when using big-Oh notation, we assume that the functions involved are (asymptotically) non-negative
- Big-Omega notation
  - Lower bounds: ``T(n)`` is ``Ω(f(n))`` if there exist constants ``c > 0`` and ``n0 ≥ 0`` such that ``T(n) ≥ c · f(n)`` for all ``n ≥ n0``
- Big-Theta notation
  - Tight bounds:  ``T(n)`` is ``Θ(f(n))`` if there exist constants ``c1 > 0``, ``c2 > 0``, and ``n0 ≥ 0`` such that ``c1 · f(n) ≤ T(n) ≤ c2 · f(n)`` for all ``n ≥ n0``
