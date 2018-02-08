# Lecture: Probabilistic Analysis, Randomised Algorithms, Universal Hashing

Slides:
  - [http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/13RandomizedAlgorithms.pdf](http://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/13RandomizedAlgorithms.pdf)
  - [http://user.it.uu.se/~pierref/courses/AD3/slides/05-ProbaAnalysisRandoAlgos.pdf](http://user.it.uu.se/~pierref/courses/AD3/slides/05-ProbaAnalysisRandoAlgos.pdf)

### Randomized algorithms
- randomization can lead to simplest, fastest, or only known algorithm for a particular problem
- guessing cards
  - shuffle a deck of ``n`` cards; turn them over one at a time; try to guess each card
  - guessing without memory (assuming player doesn't remember what's been already turned)
    - claim: the expected number of correct guesses is 1
    - Pf.:
      - let ``Xi = 1`` if the ``i``th prediction is correct, and ``0`` otherwise
      - let ``X`` = number of correct guesses = ``X1 + ... + Xn``
      - ``E[Xi] = Pr[Xi = 1] = 1/n``
      - ``E[X] = E[X1] + ... + E[Xn] = 1/n + ... + 1/n = 1``
  - guessing with memory
    - claim: the expected number of correct guesses is ``Θ(log n)``
    - Pf.:
      - let ``Xi = 1`` if the ``i``th prediction is correct, and ``0`` otherwise
      - let ``X`` = number of correct guesses = ``X1 + ... + Xn``
      - ``E[Xi] = Pr[Xi = 1] = 1/(n - (i - 1))``
        - divided by the remaining number of cards, as opposed to all of them
      - ``E[X] = E[X1] + ... + E[Xn] = 1/n + ... + 1/2 + 1/1 = H(n)``
        - ``ln(n + 1) < H(n) < 1 + ln(n)``

### Universal Hashing
- dictionary: given a universe ``U`` of possible elements, maintain a subset ``S ⊆ U`` so that inserting, deleting, and searching in ``S`` is efficient
- challenge: the universe ``U`` can be extremely large so defining an array of size ``|U|`` is infeasible
- hashing: create an array ``a`` of length ``n``. When processing element ``u``, access array element ``a[h(u)]``
- collision: when ``h(u) = h(v)`` but ``u != v``
  - a collision is expected after ``Θ(n^(1/2))`` random insertions
  - separate chaining: ``a[i]`` stores linked list of elements ``u`` with ``h(u) = i``
- deterministic hashing: if ``|U| >= n^2``, then for any fixed hash function ``h``, there is a subset ``S ⊆ U`` of ``n`` elements that all hash to same slot
- thus, ``Θ(n)`` time per lookup in worst-case
- the ideal hash function maps ``m`` elements uniformly at random to ``n`` hash slots
- approach: use randomization for choice of ``h``
  - adversary knows the randomized algorithm you are using, but doesn't know random choice that the algorithm makes
- universal hashing: a universal family of hash function is a set of hash functions ``H`` mapping a universe ``U`` to the ``{0, 1, ..., n - 1}`` such that
  - for any pair of elements ``u != v``: ``Pr_{h ⊆ H}[h(u) = h(v)] <= 1/n``
  - can select random ``h`` efficiently
  - can compute ``h(u)`` efficiently
- proposition: let ``H`` be a universal family of hash functions mapping a universe ``U`` to the set ``{0, 1, ..., n - 1}``; let ``h ⊆ H`` be chosen uniformly at random from ``H``; let ``S ⊆ U`` be a subset of size at most ``n``; and let ``u ∉ S``
- then, the expected number of items in ``S`` that collide with ``u`` is at most ``1`` (see proof using linearity of expectation in slides)

### The Hiring Problem
- an agency gives a list of ``n`` candidates
- interview them one-by-one
- after each interview, immediately decide if this candidate should be hired
- always change your mind if a better one shows up later, but that will cost
- given ``n`` candidates, how many times will we change our mind?
  - worst-case: ``n`` times
  - expected case: ?
- probabilistic analysis
  - let ``Xi = 1`` if candidate is hired, ``0`` otherwise
  - ``E[Xi] = Pr[Xi = 1] = 1/i``, where ``i`` is the number of candidates before the current candidate
  - ``E[sum_{i=1}^{n} Xi]`` = ``sum_{i=1}^{n} E(Xi) = sum_{i=1}^{n} (1/i) = ln(n) + O(1)``
- the analysis above assumed the agency's list was randomly ordered, what if not?
  - use a randomized algorithm
  - shuffle the list randomly before the interviews
  - now the probabilistic analysis holds independent of the agency's behavior
