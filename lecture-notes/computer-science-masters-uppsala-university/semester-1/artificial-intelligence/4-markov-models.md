# Lecture: Markov Models

Readings: Ch. 15, Russell, Stuart Jonathan; Norvig, Peter Artificial Intelligence: A Modern Approach 3

### Markov Models
- a Markov chain is defined by an initial state (or a distribution over the possible values for the initial state), transition probabilities (given that state at time t only depends on state at time t - 1, otherwise it would be a higher order MC). It uses nodes to represent variables and the edges to represent conditionals.

### Hidden Markov Models (HMM)
- in a hidden Markov model, the state of the system is hidden from us. This model uses some observation related to the state to infer it. The components of a HMM are the previous state, transition probabilities, and the observations.
  - inference on HMMs: forward algorithm, backward algorithm
  - most Probable Path: Viterbi Algorithm

### Markov Random Fields
- a Markov random field is a undirected network of models which specifies the conditional probability for each node given its neighbors. Inference on Markov random fields can be done by using the states we know and sampling the states we don’t know.
