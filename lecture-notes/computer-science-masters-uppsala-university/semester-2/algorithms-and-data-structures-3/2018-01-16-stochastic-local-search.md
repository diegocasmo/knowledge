# Lecture: Stochastic Local Search (SLS)

Extra Material: [Stochastic Local Search, Foundations and Applications](http://www.sls-book.net/)

Slides: [http://user.it.uu.se/~pierref/courses/AD3/slides/SLS.pdf](http://user.it.uu.se/~pierref/courses/AD3/slides/SLS.pdf)

### Local Search
- search space in constraint satisfaction problems (CSP) is huge (exponential in the number of variables)
- local search often finds a quick solution, but cannot proof there is no solution
- local search is a very general technique, and it is one of the best available methods for many CSP
- the gist:
  - consider the space of complete assignments of values to variables
  - neighbors of a current node are similar variables assignments
  - move from one node to another according to a function that scores how good each assignment is
- a local search problem consists of:
  - CSP: a set of variables, domains for these variables, and constraints on their joint values. A node in the search space will be a complete assignment to all the variables
  - neighbor relation: an edge in the search space will exist when the neighbor relation holds between a pair of nodes
  - scoring function: judges cost of a node (want to minimize)
    - E.g. the number of constraints violated in node ``n``
    - E.g. the cost of a state in an optimization context
- local search usually requires the node to be 'randomly' initialized
  - a 'random' initialization which makes sure no constraints are violated (or a subset of them) reduces the search space and improves the probability of finding a solution
- local minima
  - a minimum within some neighborhood that need not be (but may be) a global minimum
  - most research in local search concerns effective mechanism for escaping form local minima
  - want to quickly explore many local minima: global minimum is a local minimum too
- different neighborhoods
  - neighborhood: states resulting from some small incremental change to current variable assignment
  - local minima are defined with respect to a neighborhood
  - 1-exchange neighborhood:
    - one state selection: all assignments that differ in exactly one variable
      - ``O(Nd)`` = ``N`` variables, for each of them need to check ``d - 1`` values
    - two stage selection: first choose a variable (e.g. the one in the most conflicts), then best value
      - lower computational complexity, but less progress per step
  - 2-exchange neighborhood:
    - all variable assignments that differ in exactly two variables ``O(N^2 * d^2)``
    - more powerful: local optimum for 1-exchange neighborhood might no be local optimum for 2-exchange neighborhood

### Stochastic Local Search (SLS)
- uses greedy steps to find local minima
  - move to neighbor with best evaluation function value
- uses randomness to avoid getting trapped in local minima
- greedy descent is:
  - good for finding local minima
  - bad for exploring new parts of the search space
- random sampling is:
  - good for exploring new parts of the search space
  - bad for finding local minima
- a mix of the two (greedy descent and random sampling) can work very well
- only doing random steps (no greedy steps at all) is called 'random walk'
- stochastic local search for CSP
  - start: random assignment (that minimizes the number of violated constraints)
  - goal: assignment with zero unsatisfied constraints
  - heuristic function ``h``: number of unsatisfied constraints
  - a mix of:
    - greedy descent: mote to neighbor with lowest ``h``
    - random walk: take some random steps
    - random restart: reassigning values to all variables
- SLS algorithms performance can be evaluated using runtime distributions
- a SLS algorithm typically does not guarantee finding a solution even if one exists
- examples of SLS are simulated annealing and tabu search
