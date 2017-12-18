# Lecture: The Master Theorem

Readings: CLRS3 Chapter 4 (except 4.6)

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/05DivideAndConquerII.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/05DivideAndConquerII.pdf)

Instructions:
  - slides 1-12 & 25-37 of ``05DivideAndConquerII.pdf``

### The Master Theorem
- cookbook for solving recurrences of the form
  - <a href="https://www.codecogs.com/eqnedit.php?latex=T(n)&space;=&space;aT(\tfrac{n}{b})&space;&plus;&space;f(n)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?T(n)&space;=&space;aT(\tfrac{n}{b})&space;&plus;&space;f(n)" title="T(n) = aT(\tfrac{n}{b}) + f(n)" /></a>,
    - ``k = log_b(n)``
    - ``a ≥ 1`` is the (integer) number of subproblems
    - ``b ≥ 2`` is the (integer) factor by which the subproblem size decreases
    - ``f(n)`` is the work to divide and combine subproblems
  - ``T(n)`` is a function on the non-negative integers that satisfies the recurrence
- useful observations from a recursion tree
  - ``k = log_b(n)``
  - ``a^i``  = number of subproblems at level ``i``
  - ``n / b^i`` = size of subproblem at level ``i``
- given a recurrence of the form ``T(n) ≤ aT(n/b) + Θ(n^d)``
  - case 1 (total cost dominated by cost of leaves) if ``log_b(a) > d``
    - ``T(n) = Θ(n^log_b(a))``
  - case 2 (total cost evenly distributed among levels) if ``log_b(a) = d``
    - ``T(n) = Θ(n^d lg(n))``
  - case 3 (total cost dominated by cost of root) if ``log_b(a) < d``
    - ``T(n) = Θ(n^d)``
