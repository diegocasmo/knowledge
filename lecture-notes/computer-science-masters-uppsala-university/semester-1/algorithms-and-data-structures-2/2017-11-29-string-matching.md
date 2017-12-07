# Lecture: String Matching

Readings: Chapter 32, except Sections 32.3 and 32.4

Slides
  - [http://user.it.uu.se/~pierref/courses/AD2/slides/32-StringMatching-A.pdf](http://user.it.uu.se/~pierref/courses/AD2/slides/32-StringMatching-A.pdf)
  - [http://user.it.uu.se/~pierref/courses/AD2/slides/32-StringMatching-B.pdf](http://user.it.uu.se/~pierref/courses/AD2/slides/32-StringMatching-B.pdf)

### String Matching
- problem definition
  - input
    - text ``T``: 'at the thought of'
      - ``n`` = ``length(T)`` = 17
    - pattern ``P``: 'the'
      - ``m`` = ``length(P)`` = 3
    - assume ``m <= n``
  - output
    - shift ``s``: the smallest integer ``0 <= s <= n - m`` such that ``T[s .. s+m-1] = P[0 .. m-1]``
    - return ``-1`` is no such exists

### Naïve String Matching
- brute force: check all values of ``s`` from 0 to ``n-m``
- analysis
  - the analysis is made for finding all shifts
  - worst case:
    - outer loop: ``n-m+1`` iterations
    - inner loop: max ``m`` constant-time iterations
    - total: ``(n-m+1)m`` = ``O(nm)`` as ``m <= n``
    - what input given this worst-case behavior?
      - ``P=a^m`` and ``T=a^n``
      - ``P=a^(m-1)b`` and ``T=a^n``
  - best case: ``ϴ(n-m+1)``
    - ``P[0]`` not in ``T``
  - completely random text and pattern:
    - ``O(n-m)``

### Rabin-Karp Algorithm
- let the alphabet ``∑={0, 1, 2, .., 9}``
- let the fingerprint be a decimal number
  - ``f('2045')`` = ``2*10^3 + 0*10^2  + 4*10^1 + 5 = 2045``
- running time: ``2ϴ(m) + ϴ(n-m) = ϴ(n)``, as ``m <= n``
- what are the issues?
  - we cannot assume m-digit number arithmetic works in O(1) time
    - solution: hashing ``h(s)=f(s) mod q``
  - the inverse contrapositive ``f(P)=f(t)=>P=t`` was not assumed
    - when there's a hit, ``P[1 .. m] = T[s + 1 .. s + m]`` needs to be checked to rule out the possibility of a spurious hit
- basic ``mod q`` arithmetic
  - ``(a + b) mod q`` = ``(a mod q + b mod q) mod q``
  - ``(a * b) mod q`` = ``(a mod q) * (b mod q) mod q``
- analysis
  - if ``q`` is a prime number, then the hash function distributes m-digit strings evenly among the ``q`` values
    - thus, only every ``q``th value of shift ``s`` will result in matching fingerprints, which requires comparing strings with ``O(m)`` comparisons
  - expected running time, if ``q > m``
    - pre-processing: ``ϴ(m)``
    - outer loop: ``n-m+1`` iterations
    - all inner loops: maximum ``((n-m)/q)m=O(n-m)``
    - total time: ``O(n+m) = O(n)``
  - worst-case running time: ``O(nm)``
- in practice
  - if the alphabet has  ``d``  characters, then interpret characters as radix-``d`` digits: replace 10 by ``d`` in the algorithm
  - choosing a prime number ``q > m`` can be done with a randomized algorithm in ``O(m)`` time, or ``q`` can be fixed to be the largest prime so that ``d*q`` fits in a computer word
  - Rabin-Karp is simple and can be extended to two-dimensional pattern matching
