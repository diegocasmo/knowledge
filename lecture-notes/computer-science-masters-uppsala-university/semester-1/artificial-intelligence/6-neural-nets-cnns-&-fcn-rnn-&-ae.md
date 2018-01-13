# Lecture: Neural Nets, CNNs & FCN, RNN & AE

### Linear Models
- a linear model is one that outputs a weighted sum of the inputs, plus an intercept term. When there is a single input feature, X, and a single target variable, Y it produces a point in 1-dimension (no features), a line in 2-dimensions (one feature), a plane in 3-dimensions (two features), and a hyperplane in n-dimensions (n-1 features).

### Statistical Models
- in a statistical model the goal is to maximize/minimize a proposed function which is going to be used to learn the parameters of the models. For instance, we can choose to minimize the loss on the training data.

### ANNs
- its structure usually consists of an input, a hidden, and an output layer (multiple hidden layers are also possible). The hidden layers of an ANN are nonlinear feature transformations of the previous layers and the function itself is called an activation function. The transformations of an ANN are parameterized, and the values of the parameters are learn from data by  minimizing a particular loss function, such as the mean square error. A common method for this optimization is the gradient descent, which is an optimization algorithm for finding the minimum of a function. Gradient descent is usually used in conjunction with back-propagation (a common method for computing gradients).

### CNN
- a convolutional neural network is a special kind of feed-forward neural network designed to deal with spatially connected data.  A convolutional layer is similar to a normal layer, but it has a large number of shared weights and missing edges. It applies a specified number of convolution filters (a matrix of convolutional weights) to an input which produces a value in the output feature map.

### FCN
- in a FCN, each node in a layer is connected to all of the nodes in the previous layer. The output of a FCN is equivalent to sliding a CNN over larger pictures but its more efficient since it avoids a large number of redundant calculations.

### Recurrent Neural Networks (RNNs)
- a recurrent neural network is a class network which takes as input not just the current input given to it, but also what it has perceived previously in time. In other words, the decision a RNN reached at time “t-1” will affect the decision it will make a moment later at time “t”. RNN use back propagation through time to assign each weight a portion of the error when classifying input. The ability of a RNN to use what it has perceived back in time is what primarily differentiates them from a feed-forward neural network,  and as a consequence of such behavior RNNs are well suited for tasks which involve sequential input, such as hand writing recognition, speech recognition, and others.

#### Long short-term memory (LSTM)
- LSTM is a RNN architecture proposed as a solution to the problem of “forgetting” through time present in RNN. This is done by using a memory cell where information gets into it when its “write” gate is on, remains so until its “keep” gate is on, and can be read when its “read” gate is on. The gates state is determined by the neural network using a logistic and linear units. As a result of this architecture, LSTM can remember things for hundreds of time steps.

### Auto Encoders (AEs)
- AEs are a family of neural networks that are well suited for the task of figuring out the underlaying structure of a dataset. An AE takes a set of typically unlabeled inputs, and after encoding them, it tries to reconstruct them as accurate as possible. As a result, it has to figure out which of the data features are the most important, essentially acting as a feature extraction engine. AE are usually trained using back-propagation with loss, which measures the amount of information that was lost when the network tried to reconstruct the input when decoding.
