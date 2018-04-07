# Lecture: Particle Swarm Optimization

Readings: Ch 16, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### Swarm Intelligence
- intelligence can emerge from social interaction
- emergent behavior: when a group behaves in ways that were not 'programmed' into its members
- swarm intelligence
  - simulated social interaction
  - emergent collective intelligence in groups of simple agents
- bird and fish schools
  - simple local rules, a weighted combination of several goals
    - match velocity of neighbors
    - avoid collisions with neighbors
    - avoid getting too far from neighbors (or strive for center of the flock)
  - sufficient to make realistic simulations
    - used in moves and computer graphics
    - remove the match-velocity rule: insect swarm
    - remove collision rule: cultural interaction
- ants
  - ants search for shortest path through the colony (not individuals)
  - mammals build cognitive maps in their brains, ant colonies build them in their environment, through pheromone trails
  - ants are better thought as cells in a greater organism (the colony)
- stigmergy
  - indirect communication and coordination, by local modification and sensing of the environment

### Cellular Automata (CA)
- massively parallel system of identical communicating state machines
- a cell's state (on/off) is a function of the states of it communicates with its neighbors
- used to model/animate fluids, gases, bacterial growth, epidemics, etc

### Ant Colony Optimization (ACO)
- family of combinatorial optimization algorithms, based on ant behavior
- common 'real' applications: scheduling and network routing (AntNet)
- traveling salesman problem (TSP)
  - each ant ``k`` is randomly placed in a city
  - remembers the partial solution found so far (initially, the start city only)
  - moves stochastically from city ``i`` to city ``j`` by some transition probability which depends on
    - pheromone intensity
    - local information
    - whether ``j`` is valid
  - effects of alpha and beta
    - if ``alpha = 0`` and ``beta > 0``
      - pheromone information discarded, only local info used
      - stochastic greedy search with multiple starting points
    - if ``alpha > 0`` and ``beta = 0``
      - no local information used, only pheromones (more like real ants?)
      - may lead to premature convergence
        - all ants tend to follow the same (suboptimal) path
        - difficult to discover new shortcuts (as for real ants)
  - pheromone update
    - when all ants have completed a tour, let each ant deposit pheromones on the paths it followed

### Particle Swarm Optimization (PSO)
- originally intended to simulated bird flocks and to model social interaction
- a population of particles
  - typically 10-50 (smaller than EC)
- a particle ``i`` has a position ``x_i`` and a velocity ``v_i``
- each particle's position ``x_i`` represents one solution to the problem
- each particle remembers the best position it has found so far ``p_i``
- the flying particle
  - the particles 'fly' through n-dimensional space, in search for the best solution
    - ``x_i_d = x_i_d(t - 1) + v_i_d(t)``
  - the velocities ``v`` depend on previous experience of this article and that of its neighbors
  - neighborhood definition varies
    - extreme cases
      - ``pbest`` (personal)
      - ``gbest`` (global)
    - general case
      - ``lbest`` (local)
- personal best (``pbest``)
  - no interaction between particles
- global best (``gbest``)
  - global interaction
  - cognitive component: best solution found so far by this particle
  - social component: best solution found so far by any particle
- local best (``lbest``)
  - local interaction
  - ``p_l_d``: best solution found so far by any particle among its neighbors (in some structure)
- constriction
  - constrict swarm with a factor ``k``
  - to avoid divergence ('explosion')
  - no longer need a speed limit
  - lowers speed around global optimum
