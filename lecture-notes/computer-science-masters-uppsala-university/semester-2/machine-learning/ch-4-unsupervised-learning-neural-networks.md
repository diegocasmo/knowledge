# Lecture: Unsupervised Learning Neural Networks

Readings: Ch 4, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### Competitive Learning
- classification without telling which data belong to which class.
- requires that class membership can be decided by structural features in the data, and that the learning system can find these features
- K–means
  - sample (at random) ``K`` vectors from the data as initial values of ``c1, c2, ... c_K``
  - split the data in ``K`` subsets, ``D1, D2, ..., D_K``, where ``D1`` is the set of all points with ``c1`` as their closest codebook vector, ``D2`` is the corresponding set for ``c2``, etc
  - move ``c1`` towards the mean in ``D1``, ``c2`` towards the mean in ``D2``, etc
  - repeat from until convergence (until the codebook vectors stop moving)
- Voronoi regions
  - K-means define Voronoi regions in the input space
  - the Voronoi region around a codebook vector ``ci`` is the region in which ``ci`` would be the closest codebook vector
- Competitive Learning (LVQ-1 without neighborhood function)
  - the goal is to classify ``N``-dimensional data into ``M`` classes
  - the network consists of ``M`` nodes, fully connected to the inputs
  - the weight vector is a  coordinate in the N-dimensional input space (each node has a position)
  - changing the weights = moving the node
  - usual interpretation
    - given input vector, ``x``, find the node ``k`` which is closest to ``x`` (the winner, the node with the smallest distance between its weight vector and the input vector)
    - update node ``k`` so that it becomes even more likely to win next time the same input vector shows up (move it towards the input vector)
  - neural interpretation
    - find the node with the greatest weighted sum
    - update the weights of node ``k`` to increase the weighted sum for ``x`` (all other weights are left unchanged)
  - standard competitive learning rule
    - ``∆wk = η(x−wk)`` = ``η(x_i − w_{ki})`` ,``1≤i≤N``
    - only the winner is moved
- competitive learning + batch learning = K-means
- the winner-takes-all problem: if a single node wins a lot, it may become invincible
  - solutions
    - initialize the network by drawing vectors from the training set (as in K-means)
    - modify the distance measure to include the frequency of winning

### Dimensionality Reduction
- project high dimensional data into some sub-space of lower dimension without destroying the class information in the data
- Principal Component Analysis (PCA)
  - find ``M`` orthogonal vectors in the input space, in the directions of greatest variance
  - project the data down to those vectors
  - principal components are the eigenvectors of the correlation matrix of the input data, corresponding to the ``M`` largest eigenvalues
- Self Organizing Feature Maps (SOFM)
  - non-linear dimensionality reduction method which preserves the topological structure of the input space
  - nodes are organized in a two-dimensional grid
  - extension of competitive learning to update not only the winner, ``k``, but also its closest neighbors (on the grid)
  - we don't want to update the neighbors as much as the winner
    - define a neighborhood function ``f(j, k)`` which is 1 for the winner (``j = k``) and then decreases with the distance (on the grid) from node ``k`` (i.e. Gaussian)
  - learning rule
    - ``∆w_{ji} = ηf(j,k)(x_i − w_{ji})`` (for all nodes ``j`` and all inputs ``i``)
  - SOFM is often trained in two phases
    - ordering phase
      - to find the number of classes and their approximate locations
      - start with a large ``η`` and a large neighborhood
    - convergence phase
      - to decide the exact location and form of the classes
      - start with low value of ``η``
      - about ``10x`` longer training time than in the ordering phase
