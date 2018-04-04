# Lecture: Radial Basis Function Networks

Readings: Ch 5, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### Radial Basis Function Networks
- RBFNs are feed-forward networks where
  - the hidden nodes (the RBF nodes) compute distances, instead of weighted sums
  - the hidden layer activation functions are Gaussians, instead of Sigmoids
  - the output layer nodes are linear weighted sum units
  - one hidden layer only
- for classification
  - the hidden nodes form hyper spheres instead of hyperplanes
  - the weight vector of a hidden node represents the center of the sphere
  - local receptive fields (regions) instead of global dividers (an RBF node only reacts significantly for inputs in its region)
- implementation
  - each hidden node, ``j``, computes ``hj = f(rj)`` where ``rj`` is the distance between the current input vector ``x`` and the node's weight vector, ``tj`` (= the center of a hypersphere)
  - ``f(rj)`` should have a maximum at 0 (when the input vector is at the center of the sphere)
  - ``σ`` is the standard deviation (width)
- learning
  - goal: to find the position and size of the spheres (hidden layer) and how to combine them (output layer)
  - hidden nodes
    - the hidden layer positions are usually trained by competitive learning or K-means
    - their width (``σ``) are usually set to a constant value, computed after the positions have been found
    - for example, to the average distance between a node and its closest neighbor
  - output nodes
    - a layer of regular perceptrons and as such can be trained using the delta-rule

### MLP v.s. RBF
- RBFN hidden nodes form local regions, the hyperplanes of a
MLP are global
  - a RBF computes a distance between the input vector and a weigh vector, which makes the RBF discriminant a hypersphere that covers a local region
  - a MLP hidden nodes computed weighted sums,  which form hyperplanes that are infinite
  - a RBF hidden node only responds with high values for patterns close to its regions, and with very low values for patters far from that region
  - a MLP hidden node response only depends on the distance from the hyperplane, which is infinitely long, and the maximum/minimum values are for patterns infinitely far from it
- RBFNs
  - RBFNs are less sensitive to the order of presentation
  - RBFNs make less false-yes classification errors
  - RBFNs learn faster
- MLPs
  - MLPs often do better for problems with many input variables
  - MLPs often do better in regions where little data is available
  - MLPs usually require fewer hidden nodes and tend to generalize better
- in general: if data is hard to obtain, use MLP. If you have lots of data and/or if you want the network to learn continuously (on- line), use RBFN

### Regularization Techniques
- the curse of dimensionality
  - measuring distances becomes increasingly more difficult in higher dimensionality spaces
- regularization is any method which tries to prevent over-training/overfitting
- examples (explained in Ch. 7)
  - early stopping (to avoid training for too long)
  - trying to minimize the network size (parameters/weights)
  - noise injection (to force the network to generalize)
  - weight decay (with or without removal of weights)
  - Lagrangian optimization (constraint terms in the objective function)
  - multi-task learning (constrains the hidden layer)
  - averaging (over several approximations)
  - dropout
