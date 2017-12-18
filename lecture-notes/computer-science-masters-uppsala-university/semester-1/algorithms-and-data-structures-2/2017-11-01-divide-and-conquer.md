# Lecture: Divide-and-conquer

Readings: CLRS3 Chapter 4 (except 4.6)

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/05DivideAndConquerI.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/05DivideAndConquerI.pdf)

Instructions:
  - slides 1-11 of ``05DivideAndConquerI.pdf``
  - ``05DemoMerge.pdf``

### Divide-and-conquer
- the gist
  - divide up problem into several subproblems of the same kind
  - solve each problem recursively
  - combine solutions to subproblems into overall solution
- most common usage
  - divide problem of size ``n`` into two subproblems of size ``n/2`` in linear time
  - solve two subproblems recursively
  - combine two subproblems into overall solution in linear time
  - result: ``Θ(n log n)``

### Mergesort
- problem: given a list of ``n`` elements, re-arrange them in ascending order
- algorithm
  - recursively sort left half
  - recursively sort right half
  - merge two halves to make a sorted whole
- merge sort is ``O(n lg(n))``
- when creating a proof, a recursion tree can be a powerful tool to provide intuition to later on work on a formal proof (for instance, with the substitution method)
  - using recursion tree
    - ``n`` total problems to solve
    - a problem of size ``n`` can be split in two parts ``lg(n)`` times
    - ``T(n) = n lg(n)`` (assumes ``n`` is a power of 2)
  - formal proof using substitution method
    - ``T(n) = ``
      - ``0`` if ``n = 1``
      - ``2T(n/2) + n`` otherwise
    - proof
      - base case
        - when ``n = 1``, ``T(1) = 0 = n lg(n)``
      - inductive hypothesis: assume ``T(n) = n lg(n)``
      - show ``T(2n) = 2n lg 2n``
        - ``T(2n) = 2T(n) + 2n``
        - ``2n lg(n) + 2n``
        - ``2n (lg(2n) - 1) + 2n``
        - ``2n lg 2n``
