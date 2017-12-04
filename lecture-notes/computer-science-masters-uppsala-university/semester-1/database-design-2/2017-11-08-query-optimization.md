# Lecture: Query Optimization

Readings: Chapter 13, Database System Concepts, Sixth Edition (Avi Silberschatz, Henry F. Korth, S. Sudarshan).

### Introduction
- relations generated from two equivalent expressions have the same set of attributes and contain the same set of tuples
- generating a query-evaluation plans for an expression involves several steps
  - generating logically equivalent expressions (use equivalence rules to transform an expression into an equivalent one)
  - annotating resultant expressions to get alternative query plans
  - choosing the cheapest plan based on estimated cost
- the overall process is called cost based optimization
  - based on sizes of input and statistics about the attribute value distribution, estimate the cost of operations and techniques used for them
  - based on statistics about the attribute value distribution, estimate the output size of operations
- basic statistics for cost estimation
  - ``nr``: number of tuples in a relation ``r``
  - ``br``: number of blocks containing tuples of ``r``
  - ``sr``: size of a tuple of ``r``
  - ``fr``: blocking factor ``r`` (i.e. number of tuples of ``r`` that fit into one block)
  - ``V(A,r)``: number of distinct values that appear in ``r`` for attribute ``A``
  - ``SC(A,r)``: selection cardinality of attribute ``A`` of relation ``r``; average number of records that satisfy equality on ``A``

### Catalog Information for Cost Estimation
- ``HT_i``: number of levels in index ``i`` (i.e., the height of ``i``)
- for a hash index (assuming no overflow blocks), ``HT_i`` is 1.
- we refer to the cost estimate of algorithm ``A`` as ``E_A``
- equality selection
  - ``SC(A, r)``: number of records that will satisfy the selection
  - ``⎡SC(A, r)/fr⎤`` — number of blocks that these records will occupy
  - equality condition on an attribute ``A`` which is a candidate key: ``SC(A,r) = 1``
- selection involving comparisons
  - selections of the form ``σA≤V(r) `` (case of ``σA ≥ V(r)`` is symmetric)
  - let ``c`` denote  the estimated number of tuples satisfying the condition
    - if ``min(A,r)`` and ``max(A,r)`` are available in catalog
    - ``C = 0 if v < min(A,r) ``
    - ``C =  n_r * (v - min(A,r))/(max(A,r) - min(A,r))``
    - histograms can be used to improve accuracy
- estimation of the size of joins
  - let ``r``  and ``s`` be relations with ``nr*ns`` tuples; each tuple occupies ``sr + ss`` bytes respectively
  - if the join involves a key for ``r``, then a tuple of ``s`` will join with at most one tuple from ``r``
    - therefore, the number of tuples in ``r⋈s`` is no greater than the number of tuples in ``s``
  - if join is on ``S`` which is a foreign key in ``S`` referencing ``R``, then the number of tuples in ``r⋈s`` is exactly the same as the number of tuples in ``s``
- projection
  - estimated size of ``∏A(r)=V(A,r)``

### Transformation of Relational Expressions
- two relational algebra expressions are said to be equivalent if on every legal database instance the two expressions generate the same set of tuples
- in SQL, inputs and outputs are multi-sets of tuples
  - two expressions in the multi-set version of the relational algebra are said to be equivalent if on every legal database instance the two expressions generate the same multi-set of tuples
- an equivalence rule says that expressions of two forms are equivalent
- conjunctive selection operations can be deconstructed into a sequence of individual selections
  - ``σθ1^θ2(E) = σθ1(σθ2(E))``
- selection operations are commutative
  - ``σθ1(σθ2(E)) = σθ2(σθ1(E))``
- only the last in a sequence of projection operations is needed
  - ``πt1(πt2(πtn(E))) = πt1(E)``
- theta-join operations (and natural joins) are commutative
  - ``E1⋈θE2 = E2⋈θE1``
- natural join operations are associative
  - ``(E1⋈E2⋈E3 = E1⋈(E2⋈E3)  ``
- the selection operation distributes over the theta join operation under the following conditions:
  - when all the attributes in ``θ0``  involve only the attributes of one of the expressions (E1) being joined
    - ``σθ0(E1⋈θE2) = (σθ0(E1))⋈θE2 ``
- performing the selection as early as possible reduces the size of the relation to be joined

### Evaluation
- the role of the optimizer is to find a cheap evaluation plan, which is defined by an expression with annotated algorithms for each operation
- query optimizers use equivalence rules to systematically generate expressions equivalent to the given expression
- conceptually, generate all equivalent expressions by repeatedly executing the following step until no more expressions can be found
- space requirements reduced by sharing common subexpressions
- time requirements are reduced by not generating all expressions
- an evaluation plan defines exactly what algorithm is used for each operation, and how the execution of the operations is coordinated.
- choosing the cheapest algorithm for each operation independently may not yield best overall algorithm
  - merge-join may be costlier than hash-join, but may provide a sorted output which reduces the cost for an outer level aggregation
  - nested-loop join may provide opportunity for pipelining
- query optimizers incorporate elements of the following two broad approaches
  - cost-based optimization: search all the plans and choose the best plan in a cost-based fashion
  - heuristic optimization: use heuristics to choose a plan
- no need to generate all the join orders.  Using dynamic programming, the least-cost join order for any subset of  ``{r1,r2,...rn}`` is computed only once and stored for future use
- heuristic optimization transforms the query-tree by using a set of rules that typically (but not in all cases) improve execution performance:
  - perform selection early (reduces the number of tuples)
  - perform projection early (reduces the number of attributes)
  - perform most restrictive selection and join operations before other similar operations
  - some systems use only heuristics, others combine heuristics with partial cost-based optimization
- cost-based and heuristic query optimization are most commonly used in an RDBMS
