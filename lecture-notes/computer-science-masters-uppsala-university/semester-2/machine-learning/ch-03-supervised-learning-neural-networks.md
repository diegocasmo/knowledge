# Lecture: Supervised Learning Neural Networks

Readings: Ch 3, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### Neural Network Types
- a single neuron implementing summation units (SU) can be used to realize linearly separable functions only
- a layered network is required for functions that are not linearly separable
- feed-forward networks
  - in this network, the information moves in only one direction, forward, from the input nodes, through the hidden nodes (if any) and to the output nodes
  - there are no cycles or loops in the network
  - applications: classification, function approximation, perception
- recurrent networks
  - layered networks with recurrent connections between layers
  - applications: recognizing/generating sequences of patterns, linguistics
- fully interconnected recurrent networks
  - description: ??
  - applications: associative memories, combinatorial optimization problems
- multi-layer perceptron (MLP) notation N-M-K
  - ``N`` inputs
  - ``M`` hidden layers
  - ``K`` outputs

### Supervised Learning Rules
- the objective of learning is to approximate an unknown function using the given information
- a NN usually splits the data into training, validation, and test sets (all being dependent from one another)
- approximation is found via the training set, memorization is determined from the validation set, and generalization is estimated from the test set
- the hope is that a small empirical (training) error will also give small generalization error
- optimization methods
  - local optimization: the algorithm may get stuck in a local optimum without finding a global optimum (gradient descent)
  - global optimization: the algorithm searches for the global optimum by employing mechanisms to search larger parts of the search space (simulated annealing)
- learning consists of adjusting weights until an acceptable empirical error is reached
- learning algorithms based on when weights are adjusted
  - stochastic/online learning: weights are adjusted after each pattern presentation (next input pattern is randomly selected from the training set)
  - batch/offline learning: weight changes are accumulated and used to adjust weights only after training patterns have been presented (1 epoch)
  - notes
    - batch learning is the most correct, mathematically (theorems often assume this)
    - pattern learning better in practice: non-determinism reduces risk of getting trapped in local minima (usually converges faster)
- gradient descend optimization (backpropagation)
  - a learning iteration is referred to as an epoch has two phases
    - feed-forward pass: computes the output value(s) of the NN for each training pattern
    - backward propagation: propagates the error signal back from the output layer toward the input layer
  - a common improvement: momentum
    - add a momentum term to the update formula, causing smoother weight changes over time (tendency to continue moving in the same direction as before, instead of turning back and forth)
- resilient backpropagation (RPROP)
  - let the (accumulated) partial derivative decide the direction only (i.e. only consider its sign), not the step length
  - if the partial derivative changes its sign, the weight update value is decreased by a factor ``η-`` (the last weight update was too large, causing the algorithm to jump over a local minimum)
  - if the derivative retains its sign, the update value is increased by a factor of ``η+`` to accelerate convergence
  - requires batch learning

### Functioning of Hidden Units
- for classification, the task of hidden units if to form decision boundaries to separate different classes
- for function approximation, the number of inflection points plus one can be used as the number of hidden nodes to approximate a function (more may be needed)

### Ensemble Networks
- training a NN starts on randomly selected initial weights
- each time a network is retrained on the same dataset, different results can be expected
  - learning starts from different points in the search space, and may converge at different minima
- an ensemble network consists of a number of NNs all trained on the same dataset, using the same architecture and learning algorithm (this is the most basic form)
- at convergence of the individual NN members, the results of the different NNs need to be combined to form one, final result (there are different approaches to do this)

### The Perceptron As a Classifier
- the discriminant can be found by setting output to 0
- examples:
  - ``AND(1, 1, 1.5)``
  - ``OR(1, 1, 0.5)``
  - ``NAND(-1, -1, -1.5)``
  - ``NOR(-1, -1, -0.5)``
- the weights defined the position and slope of the hyperplane
- the perceptron convergence procedure
  - ``Δw_i = n(d - y)x_i`` where ``y`` is the current output and ``d`` is the desired output
    - ``n`` is the learning rate (step length)
      - larger values may overshoot minima or zig-zag across ravines
      - smaller values are slower and more likely to get stuck in local minima
    - examples:
      - ``d=y=0``, ``d - y = 0`` -> no change to the weight
      - ``d=0``, ``y=1``, ``d - y = -1`` -> total is too large, make it smaller (weaken the connections)
      - ``d=1``, ``y=0``, ``d - y = 1`` -> total is too small, make it larger (reinforce the connections)
      - ``d=y=1``, ``d - y = 0`` -> no change to the weight
  - the algorithm converges to an optimal discriminant in a finite number of steps, if such a discriminant exists (if it is linearly separable)
    - limitations:
      - XOR problem
      - solutions:
        - linearize the problem by increasing input dimensionality (e.g. an extra input which is given the AND function of the other two)
        - combine perceptrons in several layers (input -> (OR and NAND) -> AND -> output)
