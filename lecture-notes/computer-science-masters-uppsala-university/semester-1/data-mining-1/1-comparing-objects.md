# Lecture: Comparing Data Objects

Readings: Ch 2, sections 2.3 (selected parts), 2.4, Introduction to Data Mining by Tan, Steinbach and Kumar

### Comparing Data Objects
- similarity and dissimilarity
  - similarity
    - numerical measure of how alike two data objects are
    - higher when objects are more alike
  - dissimilarity
    - numerical measure of how different two data objects are
    - lower when objects are more alike
- similarity between binary vectors (compute the similarity of objects)
  - a common situation is that objects, ``p``  and ``q``, have only binary attributes
    - similarity can be computed using he following quantities
      - ``m_01``
      - ``m_10``
      - ``m_00``
      - ``m_11``
  - simple matching coefficient (counts both presence and absence equally)
    - number of matches / number of attributes
    - ``(m_00 + m_11)/(m_01 + m_10 + m_00 + m_11)``
  - Jaccard coefficients
    - number of 11 matches / number of not both zero attributes
    - ``m_11 / (m_01 + m_10 + m_11)``
  - Hamming distance
    - the number of different bits
- numerical vectors: Minkowski distance (compute the dissimilarity of objects)
  - ``r = 1`` —> Manhattan distance (``L1``)
  - ``r = 2`` —> Euclidean distance (``L2``)
  - ``r = ∞`` —> Supremum distance
    - maximum difference between any component of the vectors
- comparing text documents
  - cosine similarity
    - if ``d1`` and ``d2`` are two document vectors, then
      - ``cos(d1, d2)= (d1·d2)/||d1||*||d2||``
    - if cosine similarity is 1, then the angle between ``x`` and ``y`` is 0 (``x`` and ``y`` are the same except for magnitude)
    - if cosine similarity is 0, then the angle between ``x`` and ``y`` is 90 (``x`` and ``y`` do not share any words)
    - the cosine similarity does not take the magnitude of two data objects into account when computing similarity
- pitfalls and solutions
  - scaling
    - we need to express the different dimensions on similar or comparable ranges
    - techniques
      - min-max rescaling
        - ``(x - x_min)/(x_max - x_min)``
      - standardization
        - ``x - m_x/s_x``
          - ``m_x`` = mean
          - ``s_x`` = standard deviation
  - curse of dimensionality
    - refers to the phenomenon that many types of data analysis become significantly harder as the dimensionality of the data increases
    - when dimensionality increases, data becomes increasingly sparse in the space that it occupies
    - definitions of density and distance between points, which is critical for clustering and outliers detection, become less meaningful
  - dimensionality reduction
    - purpose
      - avoid curse of dimensionality
      - reduce amount of time and memory required by data mining algorithms
      - allow data to be more easily visualized
      - may help to eliminate irrelevant features or reduce noise
    - techniques
      - transformation of the data space
        - principal component analysis (PCA)
        - use domain knowledge
      - correlation
        - Mahalanobis
      - feature selection
        - redundant features: duplicate much or all of the information contained in one or more other attributes
        - irrelevant features: contain no information that is useful for the data mining task at hand
        - feature subset selection alternatives
          - brute-force approach: try all possible feature subsets as input to data mining algorithm
          - embedded approaches: the algorithm itself defines which attributes to use and which to ignore
          - filter approaches: features are selected before the data mining algorithm is run (independent of the data mining task)
          - wrapper approaches: use the data mining algorithm as a black box to find best subset of attributes (forward selection)
