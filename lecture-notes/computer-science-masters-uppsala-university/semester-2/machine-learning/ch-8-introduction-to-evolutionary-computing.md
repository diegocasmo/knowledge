# Lecture: Introduction to Evolutionary Computing

Readings: Ch 8, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### Evolutionary Computing (EC)
- population method (facilitates parallel search)
- used for learning problems where the task is to maximize some measure of success
- methods inspired by genetics, natural selection, evolution (but usually controlled, so it's more like breeding)
- evolutionary computing
  - genetic algorithms (GA)
    - evolutionary strategies (ES)
    - differential evolution (DE)
    - cultural evolution (CE)
  - co-evolution (predator-prey)
  - genetic programming (GP)
    - evolutionary programming (EP)
- an evolutionary algorithm
  - create a population of individuals
    - an individual is an encoding of a suggested solution to the problem
  - evaluate all individuals (for selection)
    - requires an interpretation of the encoding, and an evaluation of its fitness
  - create a new population by reproduction, recombination and/or mutation of individuals
    - during selection, fit individuals are more likely to affect the new population than non-fit ones
  - repeat from step 2 (evaluate all individuals)
- a solution to the problem is encoded by an individual's genotype (genome, chromosome), e.g. (in GA) a binary string
- a genotype's fitness (a scalar) is evaluated by a fitness function
  - requires an interpretation of the genotype (the phenotype)
  - it is the phenotype which is evaluated
  - genotype-phenotype mapping may be many-to-many (and one-to-many)
- selection
  - selection of an individual is probabilistic, based on fitness or rank
  - fitness selection: select individuals with probability proportional to their fitness
  - rank selection: select individuals with probability proportional to their rank (in a list, sorted w.r.t fitness)
  - all individuals have some probability of being selected
  - allow re-selection
- reproduction
  - select an individual (as in selection)
  - copy the selected individual, unaltered, to the new population
  - elitism: guarantee that the most fit individual(s) are reproduced this way (to avoid accidental destruction of the best solutions found so far)
- re-combination
  - select two individuals (as in selection)
  - perform a crossover of the two genotypes, creating two new genotypes/individuals
  - insert the new individual in the population
  - example:
    - 1-point crossover
      - selects a crossover point, and the bit strings after that point are swapped between the two parents
    - 2-point crossover
      - two bit positions are randomly selected, and the bit strings between these two are swapped
    - uniform crossover
      - randomly select from which point to pick a bit (or alter)
  - note: some genotypes may be illegal (breaking syntactic or semantic rules)
  - crossover 'should' be defined so that such genotypes can not be produced (an alternative could be to create a fitness function which would give a very low fitness to illegal individuals)
- mutation
  - reproduction with noise
  - some part in the copy is altered at random (e.g. in GA, by flipping bits)
  - mutation should have very low probability
    - the prime search operator in EC is usually crossover
  - mutation roughly corresponds to stochastic action selection in RL
  - some authors recommend inverted selection for mutation (higher probability to mutate low fitness individuals)

### Properties of EC
- EC is a parallel search
  - need to prevent all individuals converging to the same solution (premature convergence, a common problem)
  - mutation can help avoid this, to maintain diversity in the population (to inject new material in the population)
- EC is unlikely to get stuck in local optima
  - due to parallel search, and since it does not follow gradients
  - mutation helps
- EC does not require the objective function to be differentiable
  - the objective function is used only in selection of individuals, not how to change them
- RL is perhaps more clever in its search, but EC compensates by its more global view

## EC and Neural Networks
- EC (in particular, GA) can be a very effective way to search for the best combination of parameters to control/define something
- GA can be used to evolve, rather than train, ANNs
- could combine the two forms of learning, evolve initial network configuration and training parameters, then train the networks using neural learning algorithms
- EC does not have to follow natural laws, it could have more than two parents (Lamarckian evolution, where experience is hereditary)

### EC Issues
- generational or steady state?
- introns, useful or not?
  - introns are port of a genome which is not used (junk DNA)
- the importance of mutation
- is crossover constructive or just a form of macro mutation?
- crossover moves parts of genotypes, finding a good encoding under this operator can be difficult
  - is a substring moved from A to B still meaningful in B?
- evaluation should be simple and efficient
  - this is where EC spends most computation time
  - necessary to evaluate the whole population every generation?

### Genetic Programming (GP)
- GP = EC where the genotype is a parse tree (of a programming language)
- GP operates directly on subsets of computer programs (semi automatic programming)
- crossover and mutation in GP
  - crossover: swap randomly selected subtrees in the two parents
- many forms of mutation
  - function node: replace function with new of same arity
  - terminal node: replace terminal
  - swap: swap arguments of a function
  - grow: replace node by randomly generated subtree
  - gaussian: jog constants
  - trunc: replace node by terminal
- target languages
  - any language can be used, but Lisp is particularly suitable
  - assembler/machine code (AIM-GP)
    - assembler instruction = integers
    - store in an unsigned integer array
    - cast to function pointer and call it
  - Redcode (Corewars)
