# Lecture: Classification

Readings: Ch 4.1-4.5, 5.2, 5.7 Introduction to Data Mining by Tan, Steinbach and Kumar

### Classification
- classification is the task of assigning objects one of several predefined categories
- a key difference between classification and regression, is that the class label must be a discrete attribute for classification, as opposed to a continuous variable from regression

### Instance-Based Methods
- given a collection of records where each record contains a set of attributes and one of the attributes is the class, previously unseen records should be assigned to a class as accurately as possible (notice instance-based methods do not create a model for the classifier, they simply compare the given sample to all the dataset samples and assign a class to it)
- algorithms
  - rote-learner
    - memorize entire training data and performs classification only if attributes of record match exactly one of the training examples
  - K-NN
    - assign object the class of the closest k-records
    - parameters
      - the set of stored records
      - a proximity function
      - ``k`` the number of nearest neighbors to retrieve
    - process
      - compute distance to all training records
      - identify the ``k`` nearest neighbors
      - use class labels of nearest neighbors to determine the class label of unknown record
    - how to choose ``k``?
      - if ``k`` is too small —> sensitivity to noise points
      - if ``k`` is too large —> neighborhood may include points from other classes
      - rule of thumb ``n^(1/2)``, where ``n`` is the cardinality of the data
    - K-NN classifiers are lazy learners: they do not try to understand the data
    - complexity: ``O(ndk)``
      - ``n``: cardinality of data
      - ``d``: dimensionality of data
      - ``k``: parameter of the algorithm
    - advantages
      - can produce arbitrarily shaped decision boundaries
    - disadvantages
      - setting ``k`` may not be straightforward
      - classifying unknown records is relatively expensive (indexing might help)
      - we need a distance function

### Model-Based Classifiers
- find a model for the class attribute as a function of the values of other attributes
- a training set is used to build the model
- a test set is used to evaluate the performance of the model
- decision tree
  - Hunt's algorithm
    - for each attribute, build a tree with ``n`` leafs per node recursively
    - will only work if every combination of attribute values is present in the training data and each combination has a unique class label
  - C4.5
    - depth-first construction (like Hunt's algorithm)
    - splitting options
      - nominal attributes: one child for each value
      - ordinal and numeric attributes: sorts the values and tries the intermediate ones as splitting conditions
    - splitting criterion: gain ratio (based on entropy)
  - tree induction
    - greedy strategy: split the records based on an attribute test that optimizes certain criterion
    - how to specify the attribute test condition?
      - binary attribute
        - Test condition generates two potential outcomes (T/F)
      - nominal
        - multi-way split: Use as many partitions as distinct values
        - binary split: divides values into two subsets. Need to find optimal partitioning.
      - ordinal
        - same as nominal for multi-way split
        - same as nominal for binary split, but need to be careful about the order
      - continuous
        - discretization: transform a continuous attribute to ordinal
        - binary decision: test condition can be expressed as a comparison test ``A < v`` or ``A >= v``
    - how to determine best split?
      - greedy approach: nodes with homogeneous class distribution are preferred
      - use a node impurity formula and choose the one with the highest gain
      - measures of node impurity
        - low —> good, high —> bad
        - ``(0.5, 0.5)`` has the highest impurity
        - let ``p(i, t)`` be the frequency of class ``i`` at node ``t``
        - classification error
          - ``1 - max[p(i, t)]``
        - GINI - (CART, SLIQ, SPRINT)
          - ``1 - sum[p(i, t)^2]``
        - entropy
          - ``- sum[p(i, t) lg p(i, t)]``
      - ``GAIN_{split}`` (a.k.a. information gain when ``I`` is entropy)
        - ``I(p) - sum[ (n_i/n) I(i) ]``
          - ``I(p)``: impurity before split
          - ``n_i``: number of objects in this split
          - ``n``: total number of objects
          - ``I(i)``: impurity of this specific split
      - ``GainRATIO_{split}``
        - ``GAIN_{split}/SplitInfo``
          - where ``SplitInfo = - sum[ (n_i/n) lg (n_i/n) ]``
        - designed to overcome the disadvantages of information gain (large number of small partitions is penalized --> it favors larger nodes)
    - determine when to stop?
      - stop expanding a node when all the records belong to the same class
      - stop expanding a node when all the records have similar attribute values
      - early termination
  - advantages
    - non-parametric
    - fast
    - easy to interpret
    - robust to noise (taking care of over-fitting)
    - for many simple datasets, accuracy comparable with other methods
  - disadvantages
    - often constrained to rectilinear decision boundaries

### Pitfalls And Solutions
- under-fitting: model is too simple, both training and test error are large
- over-fitting: training error is low, but the model does not generalize well (test error is large)
  - over-fitting results in decision trees that are more complex than necessary
  - estimating generalization errors
    - re-substitute estimate (assumes the training set is a good representation of the overall data)
      - error on training ``sum[ e(t) ]``
    - generalization errors
      - error on testing ``sum[ e'(t) ]``
    - methods for estimating generalization errors
      - optimistic approach
        - ``e'(t) = e(t)``
      - pessimistic approach
          - total errors ``e'(t) = e(T) + N * 0.5``
          - ``N``: number of leaf nodes
      - reduced error pruning (REP):
        - uses validation data set to estimate generalization error
  - how to address over-fitting?
    - pre-pruning (early stop rule)
      - stop the algorithm before it becomes a fully grown tree
      - stop if all instances belong to the same class
      - stop if all the attribute values are the same
      - stop if number of instances  is less than some user-specified threshold
      - stop if expanding the current node does not improve impurity measures
    - handling over-fitting in C4.5
      - grow decision tree to its entirely
      - trim node of the decision tree in a bottom-up fashion
      - if generalization error decreases after trimming, replace sub-tree by a leaf node

### Metrics For Performance Evaluation
- focus on the predictive capability of a model
  - confusion matrix:
``` bash
           Predicted Class
         Class=Y      Class=N
Class=Y   a=TP         b=FN
Class=N   c=FP         d=TN
```
- accuracy
  - ``(a + d) / (a + b + c + d)``
  - issues with accuracy
    - over-fitting
    - over-fitting due to noise
    - class unbalance. If a particular class is underrepresented, just by classifying all instances as the overrepresented class will give the classifier a high accuracy (even though it is misleading)
      - a few solutions for this are
        - perform over/under sampling
        - use a cost matrix instead of accuracy
- cost matrix: can be used to overcome the issue of imbalance classes in a data set
- cost-sensitive measures (using the confusion matrix)
  - precision (biased towards ``C(Yes, Yes)`` and ``C(Yes, No)``)
    - ``a / (a + c)``
  - recall (biased towards ``C(Yes, Yes)`` and ``C(No, Yes)``)
    -  ``a / (a + b)``
  - F-measure (biased towards all except for ``C(No, No)``)
    - ``2a / (2a + b + c)``
  - weighted accuracy
    - ``(w1*a + w4*d) / (w1*a + w2*b + w3*c + w4*d)``

### Role of Train and Test Data
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
- bootstrap
  - a record already chosen for training is put back into the original pool of records so that it is equally likely to be redrawn
  - records that are not included in the bootstrap sample become part of the test set
