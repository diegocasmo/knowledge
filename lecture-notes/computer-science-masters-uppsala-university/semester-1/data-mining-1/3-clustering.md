# Lecture: Clustering

Readings: Ch 8.1-8.4 + a brief discussion on evaluation (read through 8.5, but no need to memorize any formulas) Introduction to Data Mining by Tan, Steinbach and Kumar

### Clustering
- finding groups of objects such that the objects in one group will be similar (or related) to one another and different from (or unrelated to) the objects in other groups
- clustering is an unsupervised task
- different types of clustering
  - partitional clustering: a division of the set of data objects into non-overlapping subsets (clusters) such that each data object is in exactly one subset
  - hierarchical clustering: set of nested clusters that are organized as a tree
  - non-exclusive clustering: an object can simultaneously belong to more than one group
  - fuzzy clustering: every object belongs to every cluster with a membership weight that is between 0 (absolutely doesn't belong) and 1 (absolutely belongs)
- different types of clusters
  - well-separated: a cluster is a set of objects in which each object is closer (or more similar) to every other object in the cluster than to any object not in the cluster
  - prototype-based: a cluster is a set of object in which each object is closer (more similar) to the prototype that defines the cluster than to the prototype of any other cluster (usually globular)
  - graph-based: a cluster can be defined as a connected component; a group of objects that are connected to one another, but that have no connection to objects outside the group
- algorithms
  - K-Means
    - definition
      - partitional clustering approach where each cluster is associated with a centroid
      - each point is assigned to the cluster with the closest centroid
      - number of clusters, ``k``, must be specified
    - solutions for initial centroids problem
      - multiple runs (with different centroids for each run, selecting the one with the lowest error)
        - helps, but probability is not on your side
      - select more than ``k`` initial centroid and then select among these initial centroids
        - select most widely separated
      - post-processing
        - use a larger ``k`` than necessary, then merge the groups
      - works well for other kinds of data (e.g., text documents)
    - details
      - the centroid is usually the mean of the points in the cluster
      - closeness is measured, e.g. by Euclidean distance
      - k-means will converge for common similarity measures
      - most of the convergence happens in the first few iterations
      - complexity: ``O(nkid)``
        - ``n``: number of points
        - ``k``: number of clusters
        - ``i``: number of iterations
        - ``d``: number of attributes
    - advantages
      - fast
      - easy to implement and parallelize
      - works well for well-separated/globular clusters
    - disadvantages
      - requires  to be specified
      - it's not robust with respect to outliers
      - it's not robust with respect to clusters
        - of different sizes
        - of different densities
        - with non-globular shapes

### Hierarchical Clustering
- produces a set of nested clusters organized as a hierarchical tree (can be visualized as a dendrogram)
- two main types of hierarchical clustering
  - agglomerative
    - start with the points as individual clusters
    - at each step, merge the closets pair of cluster until only one cluster (or  clusters) left
  - divisive
    - start with one, all-inclusive cluster
    - at each step, split a cluster until each cluster contains a point (or there are  clusters)
- traditional hierarchical algorithms use a similarity or distance metric: merge or split one cluster at a time
- algorithms
  - agglomerative clustering algorithm
    - most popular hierarchical clustering technique
    - key operation is the computation of the proximity of two clusters
      - different approaches to defining the distance between clusters distinguish the different algorithms
    - how to define inter-cluster similarity
      - MIN (single link)
        - proximity between the closest two points that are in different clusters
        - good at handling non-elliptical shapes, but is sensitive to noise and outliers
      - MAX (complete link)
        - proximity between the farthest two points in different clusters
        - less susceptible to noise and outliers, but it can break large clusters and it favors globular shapes
      - group average
        - the average pairwise proximities of all pairs of points from different clusters
        - it's a compromise between single and complete link: less susceptible to noise and outliers, biased towards globular clusters
      - distance Between Centroids
        - proximity is defined as the proximity between cluster centroids
      - other methods driven by an objective function
        - ward's method uses squared error
          - similarity of two clusters is based on the increase in squared error when two clusters are merged
          - less susceptible to noise and outliers
          - biased towards globular clusters
          - hierarchical analogue of K-means
            - can be used to initialize K-means
    - time and space
      - space ``O(n^2)``
      - time ``O(n^3)``, can be reduced to ``O(n^2 log(n))`` using a sorted-based data structure for distances
- DBSCAN
  - density-based algorithm
  - density: number of points within a specified radius (Eps)
  - core point: a point is a core point if it has more than a specified number of points (MinPts) within Eps
  - border point: a border point has fewer than MinPts within Eps, but is in the neighborhood of a core point
  - noise point: a noise point is any point that is not a core point or a border point
  - advantages
    - resistant to noise
    - can handle clusters of different shapes and sizes
  - disadvantages
    - issues with high dimensionality
    - issues with clusters with different densities
- cluster validity
- types of evaluation
  - unsupervised (internal measures)
    - cohesion: sum of the weight of all links within a cluster
    - separation: sum of the weights between nodes in the cluster and nodes outside the cluster
  - supervised (entropy)
  - relative
