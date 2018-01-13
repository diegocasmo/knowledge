# Lecture: Directed Graphical Models & MCMC

Readings: Ch. 14, Russell, Stuart Jonathan; Norvig, Peter Artificial Intelligence: A Modern Approach 3

### General Directed Graphical Models
- a network structure which encodes conditional independences between component random variables and constants and the conditional probability distributions associated with each random variable. Inference in GDGM to calculate posterior distributions of unknown variables given known variables is in general impossible, and thus the use of sampling.

### Markov Chain Monte Carlo
- generates a MC that represents the target distribution and proceed to generate samples from it by evolving the MC. As the number of samples approaches infinity, the sampled distribution approaches the actual equilibrium distribution.
- three components when the MC is evolved:
  - burn: Reduce dependency on initial state by discarding first  ``m`` samples
  - thinning: Reduce correlation of samples by taking every ``n``th sample
  - convergence: Check chain has converged by running multiple chains and checking that they are giving similar results

### Algorithms
- Gibbs: a sampling MCMC used for inference where each of the unknown variables is assigned a random value one at a time. The value is kept if the new probability if greater than the old one.
- metropolis: A sampling MCMC method used for inference where all unknown variables are updated at the same time. These values are updated by being nudged by a proposal function, and then accepted or not based on the relative probabilities of the current and proposed values.
- metropolis in Gibbs: a sampling MCMC method used to do inference where each unknown sample is randomly assigned a new value one by one (as in Gibbs). These values are updated by being nudged by a proposal function, and then accepted or not based on the relative probabilities of the current and proposed values (as in Metropolis).
