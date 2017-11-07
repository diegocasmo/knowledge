# Lecture: B+-Tree Index Files

Readings: Chapter 11, Database System Concepts, Sixth Edition (Avi Silberschatz, Henry F. Korth, S. Sudarshan).

### B+-Tree Index Files
- B+-tree indexes are an alternative t indexed-sequential files
- disadvantages of indexes-sequential files
  - performance degrades as file grows (many overflow blocks get created)
  - periodic reorganization of entire file is required
- advantages of B+-tree index files
  - automatically organizes itself with small, local, changes in the face of insertions and deletions
  - reorganization of entire file is not required to maintain performance
- B+-trees have the minor disadvantage of extra insertion/deletion overhead, and space overhead. Nonetheless, the advantages outweigh the disadvantages
- characteristics of a B+-tree index file
  - all paths from root to leaf are of the same length
  - where ``n`` is the number of pointers the node has
    - each node that is not a root nor leaf node has between ``floor(n/2)`` and ``n`` children
    - a leaf note has between ``floor(n-1)/2`` and ``n-1`` values
    - special cases
      - if root is not a leaf, it has a least ``2`` children
      - if root is a leaf, it can have between ``0`` and ``n-1`` values
- the search key in a node are ordered ``k1 < k2 < k3 < ... < kn-1``
- properties of a leaf node
  - for ``i = 1, 2, . . ., n–1``, pointer ``Pi`` points to a file record with search-key value ``Ki``,
  - if ``Li, Lj`` are leaf nodes and ``i < j``, ``Li``'s search-key values are less than or equal to ``Lj``'s search-key values
  - ``Pn`` points to next leaf node in search-key order
- non leaf nodes form a multi-level sparse index on the leaf nodes.  For a non-leaf node with ``m`` pointers:
  - all the search-keys in the subtree to which ``P1`` points are less than ``K1``
  - for ``2 ≤ i ≤ n – 1``, all the search-keys in the subtree to which ``Pi`` points have values greater than or equal to ``Ki–1`` and less than ``Ki``
  - all the search-keys in the subtree to which ``Pn`` points have values greater than or equal to ``Kn–1``
- insertions and deletions to the main file can be handled efficiently, as the index can be restructured in logarithmic time
- a node is generally the same size as a disk block, typically 4 kilobytes
- example of operations on B+-trees:
  - [B+-Trees basics](https://www.youtube.com/watch?v=CYKRMz8yzVU)
  - [B+ Trees insertion)](https://www.youtube.com/watch?v=_nY8yR6iqx4&t=198s)
  - [B+ Trees deletion)]](https://www.youtube.com/watch?v=QrbaQDSux M)
