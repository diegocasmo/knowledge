# Lecture: Fundamentals of Integer Programming

Extra Material: [Integer Programming](https://www.wiley.com/en-gb/Integer+Programming-p-9780471283669)

Slides: [http://user.it.uu.se/~pierref/courses/AD3/slides/MIP.pdf](http://user.it.uu.se/~pierref/courses/AD3/slides/MIP.pdf)

### Optimization and Programming
- mathematical programming: to find the best solution from a set of alternatives
- a generic formulation defines an objective function which needs to be minimized/maximized, a solution set, and constraints which need to be satisfied (if any)
- the variables (their values) represent problem solution
- the solution set is specified by equations and inequalities
- example: binary knapsack
  - a set of items, each with a value and weight
  - a knapsack with a weight capacity limit
  - goal: select items to maximize the total value of the knapsack without exceeding the weight limit
  - solution:
    - ``$max(\sum_{i=1}^{n} c_i x_i)$``
    - such that
      - ``$\sum_{i=1}^{n} a_i x_i \leq b$``
      - ``$x \in {0, 1}$``, ``$i = 1, ..., n$``
- other examples of mixed integer (linear) programming (MIP) can be found on the slides: coloring, set covering, uncapacitated facility location, and the traveling salesman problem
- types of optimization models
  - linear programming
  - integer (linear) programming
  - nonlinear programming
  - integer nonlinear programming
- linear programming relaxation
  - relaxation: 'removal' of some constraints/restrictions
  - in general, the linear programming relaxation is an approximation of the integer model; the solution of the former may be fractional
- solving a linear programming model
  - fundamental property: optimum is located at one of the extreme/corner points
- convex hull: the minimum convex set containing the solution space
- integer programming = linear programming on the convex hill of the integer points
- convex hull exists, but its description is hard to derive in general
- optimality gap: the relative difference between the objective value of the best known integer solution and that of the best optimistic LP bound so far
