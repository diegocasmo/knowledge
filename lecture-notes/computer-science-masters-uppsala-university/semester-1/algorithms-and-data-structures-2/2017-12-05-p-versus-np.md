# Lecture: P versus NP

Readings: CLRS3 Chapter 34

Slides: [http://user.it.uu.se/~pierref/courses/AD2/slides/34-PversusNP.pdf](http://user.it.uu.se/~pierref/courses/AD2/slides/34-PversusNP.pdf)

### Introduction
- informal definition
  - P = class of problems that need no search to be solved
  - P = class of problems with easy-to-compute solutions
  - NP = class of problems that might need search to solve
  - NP = class of problems with easy-to-check solutions
- problems solvable in polynomial time are considered tractable, or easy. Problems requiring super-polynomial time are considered intractable, or hard
- question: can brute-force search always be avoided (P = NP),
or is brute-force search sometimes necessary (P ̸= NP)?
- more formally, and focusing on decision problems, whose answer is 'yes' or 'no', for inputs of size ``n``
  - P = the class of easy problems, whose solutions can be computed in polynomial time: ``O(n^k)`` for some fixed ``k``
    - examples: sorting; almost all problems in this course
  - NP = the class of problems for which a witness can be checked in polynomial time, when the answer is 'yes'.
    - NP stands for "non-deterministic polynomial time"
    - we trivially have ``P ⊆ NP``
    - example (factoring): Given a 1,000-digit number, does it have a divisor ending in 7? Computing such a divisor seems hard, but checking a candidate divisor is easy
  - undecidable problems cannot be solved by any algorithm, no matter how much time is allocated
    - examples: halting problem; disjointness of two CFLs
    - so not all problems are in NP, independently of P versus NP

### Reduction and NP Hardness
- informally
  - a problem ``Q`` reduces to a problem ``R``, denoted ``Q ≤P R``, if every instance of ``Q`` can be transformed in poly-time into an instance of ``R`` that has the same yes/no answer.
  - we also say that ``R`` is at least as hard as ``Q``. Note that ``≤P`` is transitive: ``∀Q,E,R: Q ≤P E ≤P R ⇒ Q ≤P R``
  - proving that a problem ``Q`` is in ``P`` is done by showing ``Q ≤P E`` for some existing problem ``E`` in ``P``
  - a problem is NP-hard if it is at least as hard as every problem in NP: every problem in NP reduces to it
- formally
  - a problem is NP-complete if it is in NP and is NP-hard
  - if some NP-complete problem is polynomial-time solvable, then every problem in NP is poly-time solvable: ``P ⊇ NP``
  - an NP-complete problem is poly-time solvable iff ``P = NP``
  - if some problem in NP is not poly-time solvable (``P =! NP``), then no NP-complete problem is polynomial-time solvable
  - the status of NP-complete problems is currently unknown
    - no polynomial-time algorithm was found for any of them, and no proof was made that no such algorithm can exist
    - most experts believe NP-complete problems are intractable, as the opposite would be truly amazing
