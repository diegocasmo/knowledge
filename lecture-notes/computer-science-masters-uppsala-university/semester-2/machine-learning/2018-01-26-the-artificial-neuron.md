# Lecture: The Artificial Neuron

Readings: Ch 2, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### The Artificial Neuron (AN)
- an artificial neuron receives a vector of input signals (either from the environment or from other AN)
- to each input signal there is a weight associated which strengthens or depletes the input signal
- the strengthen of the output is also influence by a bias (threshold) term
- the net input signal of an AN is usually computed as the weighted sum of all input signals

### Activation Functions
- an activation function receives the input signal and bias, and determines the output (or firing strength) of a neuron
- frequent activation functions (see plots of each function in book)
  - linear: produces a linearly modulated output where lambda is a constant
  - step function: produces one of two scalar output values, depending on the value of the bias
  - ramp function: a combination of the linear and step function
  - sigmoid function: a continuous version of the ramp function
  - hyperbolic tangent
  - Gaussian function

### Artificial Neuron Geometry
- a single neuron can be used to realize linearly separable functions without any error
  - examples of these are the Boolean functions AND and OR
- to be able to learn function that are not linearly separable, a layered NN of several neurons is required
  - an example of this is XOR

### Artificial Neuron Learning
- learning allows to automatically learn the value of the weights and the bias term
- learning consists of adjusting weights and bias until a certain criterion is met
- three main types of learning
  - supervised learning: neuron is provided with a data set consisting of input vectors and a target associated with each input vector
  - unsupervised learning: aim is to discover patterns or features in the input data with no assistance from an external source
  - reinforcement learning: the neuron is rewarded for good performance and penalized for bad performance

### Augmented vectors
- gradient descent is one of the most common learning rules
- gradient descent requires the definition of an error (or objective) function to measure the neuron's error in approximating the target
  - a popular option is the sum of squared errors
- the goal of gradient descent is to find the weight values that minimize the total error
