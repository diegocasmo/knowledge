# Lecture: Query Processing

Readings: Chapter 12, Database System Concepts, Sixth Edition (Avi Silberschatz, Henry F. Korth, S. Sudarshan).

### Query Processing
- query tree notation
  - a query tree is an internal data structure to represent a query
  - a standard technique for estimating the work involved in executing a query
  - nodes represent operations such as selection, projection, join, etc
  - leaf nodes represent base relations
- algebraic query optimization consists of rewriting the query or modifying the query tree into an equivalent tree
- different algorithm exists for handling a query, these are based on the way files are organized and the existence of indexes
- basic steps in query processing
  - parsing and translation
    - translate query into its internal form
    - parser checks syntax, verifiers relations
  - optimization
    - amongst all equivalent evaluation plans, choose the one with lowest cost
    - cost is estimated using statistical information from the database catalog
  - evaluation
    - the query execution engine takes a physical query plan (aka execution plan), executes the plan, and returns the result
- a relation algebra may have many equivalent expressions
  - each relation algebra operation can be evaluated using one of several algorithms
- annotated expression specifying detailed evaluation strategy is called an ``evaluation-plan``
- the cost is generally measured as the total elapsed time for answering a query
  - typically disk access is the predominant cost and relatively easy to estimate
    - ``number of seeks * average-seek-cost``
    - ``number of blocks read * average-block-read-cost``
    - ``number of blocks written * average-block-write-cost``
  - cost to write a block is greater than cost to read a block
    - data is read back after being written to ensure that the write was successful
  - for simplicity,we just use the number of block transfers from disk as the cost measure
    - ``b * t_T`` (number of blocks transfer * time to transfer a block)
    - but we will not consider ``t_T``; we'll only consider ``b``

### Algorithms for Query Processing
- file scan
  - A1: scan each file block and test condition
    - cost: ``b_r`` transfers
- index scan
  - A2 (primary index, equality on key): retrieve a single record that satisfies the equality condition
    - cost: ``h_i`` + 1 (``h_i`` is the height of the B+-tree)
  - A3 (primary index, equality on nonkey): retrieve multiple records
    - cost: ``h_i + b`` (``b`` is the number of blocks containing matching records)
  - A4 (secondary index, equality on nonkey)
    - retrieve a single record if the search-key is a key
      - cost: ``h_i + 1``
    - retrieve multiple records if a search key is not a key
      - cost: ``h_i + n`` (where ``n`` is the number of records retrieved)
- selection involving comparisons
  - A5 (primary index, comparison)
    - for ``σA ≥ V(r)``  use index to find first tuple ``≥ v``  and scan relation sequentially  from there
    - for ``σA≤V (r)`` just scan relation sequentially till first tuple ``> v``; do not use index
  - A6 (secondary index, comparison)
    - for ``σA ≥ V(r)``  use index to find first index entry ``≥ v`` and scan index sequentially  from there, to find pointers to records
    - for ``σA≤V (r)`` just scan leaf pages of index finding pointers to records, till first entry ``> v``
  - A7 (conjunctive selection using one index)
    - select a combination of ``θi`` and algorithms A1 through A7 that results in the least cost for ``σθi (r)``

### Sorting
- sorting of data plays an important role in database systems for two reasons
  - first, SQL queries can specify that the output be sorted
  - second, and equally important for query processing, several of the relational operations, such as joins, can be implemented efficiently if the input relations are first sorted
- for relations that fit in memory, techniques like quick-sort can be used. For relations that don't fit in memory, external sort-merge is a good choice
- see slides for examples to practice how to do external-sort merge

### Join Operation
- several algorithms to implement joins
  - nested-loop join
    - outer relation called ``r`` and inner relation called ``s``
    - requires no indexes and can be used with any kind of join condition
    - expensive since it examines every pair of tuples in the two relations
    - cost = ``b_r + n_r*b_s``
      - ``n_r`` = # tuples in ``r``
      - ``b_r`` = # blocks in ``r``
      - ``b_s`` = # blocks in ``s``
    - will perform better if inner relation is smaller than the outer one
  - block nested-loop join
    - variant of nested-loop join in which every block of inner relation is paired with every block of outer relation
    - cost = ``b_r + b_r*b_s``
  - indexed nested-loop join
    - index lookups can replace file scans if join is an equi-join or natural join and an index is available on the inner relation's join attribute
    - cost = ``b_r + n_r*index_search_cost``
  - merge join
    - sort both relations on their join attribute (if not already sorted on the join attributes)
    - merge the sorted relations to join them
    - merge join can be used only for equi-joins and natural joins
    - cost = ``ext.sorting(r) +  ext.sorting(s) +   b_r + b_s``

### Evaluation of Projections
- projection: drop some columns of the input and eliminate duplicate tuples
- duplicate elimination is the most expensive part of the projection operation
- if input of projection is a relation file and there is an index on projection attributes, we can use it and perform duplicate elimination at one scan of the index (duplicates are in the same or neighboring index pages)
- if no index exists for the input of projection, then we must perform partitioning  sorting or hashing) to bring duplicates together and eliminate them

### Evaluation of Expressions
- alternatives for evaluating an entire expression tree
- materialization
  - generate results of an expression whose inputs are relations or are already computed, materialize (store) it on disk
  - evaluate one operation at a time, starting at the lowest-level. Use intermediate results materialized into temporary relations to evaluate next-level operations
  - materialized evaluation is always applicable
  - cost of reading-writing results from-to disk can be quite high
- pipelining
  - pass on tuples to parent operations even as an operation is being executed
  - evaluate several operations simultaneously, passing the results of one operation on to the next
  - much cheaper than materialization: no need to store a temporary relation to disk
  - pipelining may not always be possible – e.g., sort, hash-join
