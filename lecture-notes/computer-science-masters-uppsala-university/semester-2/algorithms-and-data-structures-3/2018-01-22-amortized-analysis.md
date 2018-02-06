# Lecture: Amortized Analysis

Slides: [http://user.it.uu.se/~pierref/courses/AD3/slides/17-AmortisedAnalysis.pdf](http://user.it.uu.se/~pierref/courses/AD3/slides/17-AmortisedAnalysis.pdf)

### Amortized Analysis
- worst-case analysis can be too pessimistic if the only way to encounter an expensive operation is when there were lost of previous cheap operations
- amortized analysis: determine worst-case running time of a sequence of ``n`` data structure operations
- three different methods:
  - aggregate method (brute force)
  - accounting method (banker's method)
    - assign (potentially) different charges to each problem
    - ``D_i`` is data structure after ``i^th`` operation
    - ``c_i`` actual cost of the ``i^th`` operation
    - ``ĉ_i`` amortized cost of the ``i^th`` operation = amount we charge operation ``i``
    - when ``ĉ_i > c_i`` we store credits in data structure ``D_i`` to pay for future ops; when ``ĉ_i < c_i``, we consume credits in data structure ``D_i``
    - initial data structure ``D_0`` starts with ``0`` credits
    - credit invariant: the total number of credits in the data structure ``>= 0``
      - ``sum_{i=1}(ĉ_i) - sum_{i=1}(c_i) >= 0``
    - theorem: starting from the initial data structure ``D_0``, the total actual cost of any sequence of ``n`` operations is at most the sum of the amortized costs
      - Pf. ``sum_{i=1}(ĉ_i) >= sum_{i=1}(c_i)``
  - the potential method (physicist's method)
    - ``θ(D_i)`` maps each data structure ``D_i`` to a real number s.t:
      - ``θ(D_0) = 0``
      - ``θ(D_i) >= 0`` for each data structure ``D_i``
    - ``c_i`` actual cost of the ``i^th`` operation
    - ``ĉ_i = c_i + θ(D_i) - θ(D_{i - 1})`` = amortized cost of ``i^th`` operation
    - theorem: starting from the initial data structure ``D_0``, the total actual cost of any sequence of ``n`` operations is at most the sum of the amortized costs

### Binary Counter
- goal: increment a ``k``-bit binary counter (``mod 2^k``)
- aggregate method
  - analyze cost of a sequence of operations
  - starting from the zero counter, in a sequence of ``n`` ``INCREMENT`` operations
    - bit ``0`` flips ``n`` times
    - bit ``1`` flips ``floor(n/2)`` times
    - bit ``2`` flips ``floor(n/4)`` times
  - theorem: starting from the zero counter, a sequence of ``n`` ``INCREMENT`` operations flips ``O(n)`` bits
    - bit ``j`` flips ``floor(n/2^j)`` times
    - total number of bits flipped is:
      - ``sum_{j=0}^{k-1} floor(n / 2^j)``
      - ``n * sum_{j=0}^{k-1} 1 / 2^j``
      - ``2n``
- accounting method
  - credits: one credit pays for a bit flip
  - invariant: each ``1`` bit has one credit; each ``0`` bit has zero credits
  - accounting:
    - flip bit ``j`` from ``0`` to ``1``: charge ``2`` credits (use one and save one in bit ``j``)
    - flip bit ``j`` from ``1`` to ``0``: pay for it with the ``1`` credit saved in bit ``j``
  - theorem: starting from the zero counter, a sequence of ``n`` ``INCREMENT`` operations flips ``O(n)`` bits
  - Pf.
    - each ``INCREMENT`` operations flips at most one ``0`` bit to a ``1`` bit, so the amortized cost per ``INCREMENT`` ``<= 2``
    - invariant: the number of credits in data structure ``>= 0``
    - total actual cost of ``n`` operations ``<=`` sum of the amortized costs ``<=`` ``2n``
- potential method
  - let ``θ(D)`` = number of ``1`` bits in the binary counter ``D``
  - ``θ(D_0) = 0``
  - ``θ(D_i) >= 0`` for each ``D_i``
  - theorem: starting from the zero counter, a sequence of ``n`` ``INCREMENT`` operations flips ``O(n)`` bits
  - Pf.
    - suppose that the ``i^th`` ``INCREMENT`` operation flipts ``t_i`` bits from ``1`` to ``0``
    - the actual cost ``c_i <= t_i + 1``
    - the amortized cost ``ĉ_i = c_i + θ(D_i) - θ(D_{i-1})``
      - ``ĉ_i <= c_i + 1 - t_i``
      - ``ĉ_i <= t_i + 1 + 1 - t_i``
      - ``ĉ_i <= 2``
    - the actual cost of ``n`` operations ``<=`` sum of amortized costs ``<=`` ``2n``

### Other Examples
  - CLRS3 also explains the binary counter example, plus a multi-pop stack, and dynamic tables (all of them using each of the three techniques described above)
