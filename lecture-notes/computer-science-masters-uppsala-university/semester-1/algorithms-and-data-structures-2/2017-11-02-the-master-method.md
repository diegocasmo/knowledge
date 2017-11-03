# Lecture: The Master Theorem

Readings: CLRS3 Chapter 4 (except 4.6)

Slides: [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/05DivideAndConquerII.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/05DivideAndConquerII.pdf)

### The Master Theorem
- 'Cookbook' for solving recurrences of the form
  - <a href="https://www.codecogs.com/eqnedit.php?latex=T(n)&space;=&space;aT(\tfrac{n}{b})&space;&plus;&space;f(n)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?T(n)&space;=&space;aT(\tfrac{n}{b})&space;&plus;&space;f(n)" title="T(n) = aT(\tfrac{n}{b}) + f(n)" /></a>,
    - ``a ≥ 1`` is the (integer) number of subproblems
    - ``b ≥ 2`` is the (integer) factor by which the subproblem size decreases
    - ``f(n)`` is the work to divide and combine subproblems
  - ``T(n)`` is a function on the non-negative integers that satisfies the recurrence
- Useful observations from a recursion tree
  - <a href="https://www.codecogs.com/eqnedit.php?latex=k&space;=&space;log_ba" target="_blank"><img src="https://latex.codecogs.com/gif.latex?k&space;=&space;log_ba" title="k = log_ba" /></a> levels
  - ``a^i``  = number of subproblems at level ``i``
  - ``n / b^i`` = size of subproblem at level ``i``
- There are three cases in the master theorem (check [Stanford slides](https://web.stanford.edu/class/archive/cs/cs161/cs161.1138/lectures/07/Small07.pdf) for another explanation of it)
  - Case 1
    - if <a href="https://www.codecogs.com/eqnedit.php?latex=f(n)&space;=&space;O(n^{log_ba-&space;c})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f(n)&space;=&space;O(n^{log_ba-&space;c})" title="f(n) = O(n^{log_ba- c})" /></a> for some constant ``c > 0``, then <a href="https://www.codecogs.com/eqnedit.php?latex=T(n)&space;=&space;\Theta(n^{log_ba})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?T(n)&space;=&space;\Theta(n^{log_ba})" title="T(n) = \Theta(n^{log_ba})" /></a>
    - total cost dominated by cost of leaves
  - Case 2
    - if <a href="https://www.codecogs.com/eqnedit.php?latex=f(n)&space;=&space;\Theta(n^{log_ba})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f(n)&space;=&space;\Theta(n^{log_ba})" title="f(n) = \Theta(n^{log_ba})" /></a>, then <a href="https://www.codecogs.com/eqnedit.php?latex=T(n)&space;=&space;\Theta(n^{log_ba}lg&space;(n))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?T(n)&space;=&space;\Theta(n^{log_ba}lg&space;(n))" title="T(n) = \Theta(n^{log_ba}lg (n))" /></a>
    - total cost evenly distributed among levels
  - Case 3
    - if <a href="https://www.codecogs.com/eqnedit.php?latex=f(n)&space;=&space;\Omega(n^{log_ba&space;&plus;&space;c})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f(n)&space;=&space;\Omega(n^{log_ba&space;&plus;&space;c})" title="f(n) = \Omega(n^{log_ba + c})" /></a> for some constant ``c > 0``, and if <a href="https://www.codecogs.com/eqnedit.php?latex=af(\tfrac{n}{b})&space;\le&space;cf(n)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?af(\tfrac{n}{b})&space;\le&space;cf(n)" title="af(\tfrac{n}{b}) \le cf(n)" /></a> for some constant ``c < 1`` and all sufficiently large ``n``, then <a href="https://www.codecogs.com/eqnedit.php?latex=T(n)&space;=&space;\Theta(f(n))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?T(n)&space;=&space;\Theta(f(n))" title="T(n) = \Theta(f(n))" /></a>
    - total cost dominated by cost of root
