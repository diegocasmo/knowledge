# Lecture: Performance Issues (Supervised Learning)

Readings: Ch 7, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### Performance Measures
- generalization is a measure of how well the network performs on previously unseen data
  - the ultimate objective of NN learning it to produce a learner with low generalization error
- overfitting occurs when a NN exhibits a low training error, but bad generalization error. Consequently, the NN memorizes the training patterns and loses the ability to generalize
  - it can happen when:
    - network is trained for too long and the excess free parameters start to memorize all the training patterns (even noise contained in the training set)
    - the NN architecture is too large (NN has too many weights, ~ too many free parameters)
- the training set is used to compute weight changes
- the validation set is used to plot the error during training (on the training set), in order to find a minimum
- the performance (the results of the experiment) is then measured using the test set (generalization set)
- for classification problems, the percentage correctly classified patterns is used as a measure of accuracy
  - mean square error (MSE) is not a good measure for classification because the network may have a good accuracy in terms of the number of correct classifications, while having a relatively large MSE
- complexity: the computational complexity of a NN is directly influenced by
  - the network architecture: the larger the architecture, the more feed-forward calculations are needed to predict outputs after training, and the more learning calculations are needed per patter presentation
  - the training set size: the total number of learning calculations per epoch is increases as the training size increases
  - complexity of the optimization method: increases computation complexity to determine the weight updates
- holdout
  - original data is partitioned into two disjoint sets, the training and data set
  - limitations
    - fewer labeled examples available for training
    - model highly dependent on the composition of the training and test sets
    - training and test data sets are no longer independent (an over-represented subset in one, will be underrepresented on the other, or vice versa)
- cross-validation
  - each record is used the same number of times for training, and exactly once for testing
  - the total error is found by summing up the errors for all ``k`` runs
- leave-1-out
  - a special case of ``k``-fold cross-validation where ``k=N``, the size of the dataset
  - each test set contains only one record
  - limitations
    - computationally expensive
    - variance is usually high
- dropout
  - at each training stage, individual nodes are either 'dropped out' of the net with probability ``1 − p`` or kept with probability ``p``, so that a reduced network is left; incoming and outgoing edges to a dropped-out node are also removed. Only the reduced network is trained on the data in that stage. The removed nodes are then reinserted into the network with their original weights
- the number of training examples should be much larger than the number of weights (``2N...N^2``)

### Performance Factors
- data-preparation
  - missing values
    - remove the entire pattern if it has a missing value (reduces available information for training and important information may be lost)
    - replace each missing value with the average value (continuous values) or most frequently occurring value (nominal or discrete values)
- an outlier is a pattern that deviates substantially from the data distribution
  - outliers result in large errors, and consequently large weight updates
- scaling and normalization: while it is not necessary to scale input values, performance can be improve if inputs are spaced to the active domain of the activation functions
- noise injection: add a small amount of noise to the input values every time an input pattern is presented to the learning system, so that the patterns never look quite the same. It forces the learning system to find more general representations.
- weight initialization
  - gradient descent is very sensitive to the initial weight vectors
  - if the initial position is close to a local minimum, convergence will be fast
  - if the initial weight vector is on a flat area in the error surface, convergence is slow
  - weights need to be initialized to a small random non-zero values because otherwise all the units will produce the same output, and thus make the same contribution to the approximation error. all the weight are therefore adjusted with the same value (weights will remain the same irrespective of training time - hence, no learning takes place)
- learning rate
  - the learning rate controls the size of each step towards the minimum of the objective function
  - if it is too small, the weight adjustments are correspondingly small and more iterations are required to reach a local minimum
  - if too large, the weight adjustments are correspondingly large. Convergence will initially be fast, but the algorithm will eventually oscillate without reaching the minimum (it could also cause it to jump a good local minimum and proceed towards a bad local minimum)
  - heuristics to adjust learning rate
    - assume each weight has a different learning rate
      - if the direction in which the error decreases at this weight change is same as the direction in which it has been decreasing recently, then the learning rate is increased; if not, it is decreased
      - the direction in which the error decreases is determined by the sign of the partial derivative of the objective function with respect to the weight
    - an alternative is to use an annealing schedule to gradually reduce a large learning rate to a smaller value
- momentum
  - the idea of the momentum term is to average the weight changes, thereby ensuring that the search path is in the average downhill direction
  - the momentum term is then simply the previous weight change weighted by a scalar value ``α``
- when performing gradient descent, learning rate measures how much the current situation affects the next step, while momentum measures how much past steps affect the next step

### Network size
- with sufficiently many hidden layers and nodes, the MLP can approximate any function to any degree of accuracy
  - one hidden layer is sufficient. However, the # of required nodes in that layer may be very large (``+∞``)
  - two hidden layers are therefore sometimes used, which drastically reduces the required number of nodes in each layer
- the effects of hidden nodes
  - classification: the hidden nodes form discriminants
  - function approximation: the hidden nodes correspond to monotonic regions in the function
- note: the ability to represent a function is not a guarantee that this function can be found by training (the required number of hidden nodes may be greater in practice than in theory)
- ANNs for classification should have sigmoid outputs, while ANNs for functions approximation should have linear outputs
  - a linear output is required for function approximation since we do not know the range of the target function, and even if we do it is unlikely that the range happens to be the usual sigmoid [0, 1]. A function approximator should be able to output any value.

### Architecture Selection
- the goal is to minimize the number of hidden nodes (less parameters) better generalization
  - less over-training, faster recall, faster training
  - if several networks fit the training set equally well, then the simplest network will on average give the best generalization performance
- two approaches
  - start with a large net and prune
  - start with a small net and extend
- regularization
  - addition of a penalty term to the objective function to be minimized ``E = Et + λ*Ec``
  - where ``Et`` is the usual measure of data error, and ``Ec`` is a penalty term, penalizing network complexity (network size)
  - the constant ``λ`` controls the influence of the penalty term
  - weight decay
    - an instance of a regularization method
    - the network cuts unnecessary connections automatically
    - let each weight strive to zero. only the most important weights will stay up
- network construction (growing)
  - network construction algorithms start training with a small network and incrementally add hidden units during training when the network is trapped in a local minimum
  - the upstart algorithm (only for classification)
    - an instance of a network construction growing algorithm
    - an output node in a classifier network can make two types of mistakes
      - ``E+``: The node's value is 1, when it should be 0
      - ``E-``: The node's value is 0, when it should be 1
    - for each output node, create two daughter nodes, ``x+`` and ``x-``, separately trained to recognize the cases where the parent node make mistakes of type ``E+`` and ``E-``, respectively
      - ``x+`` is connected to the parent node with a large negative weight
      - ``x-`` is connected to the parent node with a large positive weight
    - if the daughters cannot solve this task, let them create their own daughters, etc
- network pruning
  - start with an over-sized network and remove unnecessary network parameters, either during training or after convergence to a local minimum
  - the decision to prune a network parameter is based on some measure of parameter relevance or significance

### Second Order Methods
- use second order information to guess where the minimum is, then jump directly there
- example: quickprop
  - requires batch learning
  - for every weight ``w_ji`` assume that
    - the error surface can be approximated locally by a parabola
    - the change in the slope from the previous step is only due to ``w_ji``
  - given this, the current and previous slope together with the latest weight change can be used to define a parabola -> jump directly to the minimum of that
  - sensitive to choice of parameters, but can be be extremely fast
- no free lunch theorem
  - averaged over all possible learning problems, no learning algorithm is better than any other
  - implication
    - there is always a catch: all modifications that seem to make things better, must have a drawback
    - if your algorithm is worse than another on a subset of problems, you also know that there is another subset for which your algorithm is better (but is it an interesting subset?)

### Multi-task Learning
- have additional data which we think could help the networks
- extra info can make network dependent on it
- instead, add it as an extra output
- this restricts the freedom of the hidden layer, i.e. the number of models the network can form of the target function.
- results in faster training, better generalization (in practice, not guaranteed), less variance in both training time and results
