# Lecture: Supervised Learning Neural Networks

Readings: Ch 3, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### Neural Network Types
- a single neuron implementing summation units (SU) can be used to realize linearly separable functions only
- a layered network is required for functions that are not linearly separable
- feed forward neural networks (FFNN)
  - the output of a FFNN is computed with a single forward pass through the network
  - activation functions of a neural network do not need to all be the same
- functional link neural network (FLNN)
  - input units do implement activation functions
  - a FLNN is a FFNN with the input layer expanded into a layer of functional high-order units
  - the use of high-order combinations of input units may result in faster training times and improved accuracy
- product unit neural networks (PUNN)
  - computes the weighted product of input signals, instead of a weighted sum
- simple recurrent neural networks (SRNN)
  - has feedback connections which add the type ability to also learn the temporal characteristics of the data set
  - the previous state of the output layer then also serves as input to the network
- time-delay neural networks (TDNN)
  - TDNN are also refereed to as backpropagation through-time
  - its input patterns are successively delayed in time
- cascade networks
  - is multilayer FFNN where all input units have direct connections to all hidden units and to all output units
  - each hidden unit's output serves as an input to all succeeding hidden units and all output units

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
  - stochastic/online learning: weights are adjusted after each patter presentation (next input pattern is randomly selected from the training set)
  - batch/offline learning: weight changes are accumulated and used to adjust weights only after training patterns have been presented
- gradient descend optimization (backpropagation)
  - a learning iteration is referred to as an epoch has two phases
    - feed-forward pass: computes the output value(s) of the NN for each training pattern
    - backward propagation: propagates the error signal back from the output layer toward the input layer

### Functioning of Hidden Units
- for classification, the task of hidden units if to form decision boundaries to separate different classes
- for function approximation, the number of inflection points plus one can be used as the number of hidden nodes to approximate a function (more may be needed)

### Ensemble Networks
- training a NN starts on randomly selected initial weights
- each time a network is retrained on the same dataset, different results can be expected
  - learning starts from different points in the search space, and may converge at different minima
- an ensemble network consists of a number of NNs all trained on the same dataset, using the same architecture and learning algorithm (this is the most basic form)
- at convergence of the individual NN members, the results of the different NNs need to be combined to form one, final result (there are different approaches to do this)
