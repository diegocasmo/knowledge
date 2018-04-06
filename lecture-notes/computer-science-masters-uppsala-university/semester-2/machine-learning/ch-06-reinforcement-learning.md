# Lecture: Reinforcement Learning

Readings: Ch 6, Computational Intelligence: An Introduction, Andries P. Engelbrecht

### Reinforcement Learning (RL)
- RL is the learning of a mapping from situations to action with the main objective to maximize the scalar reward or reinforcement signal (i.e. learning by trial-and-error)
- typical reinforcement problem:
  - agent receives sensory inputs from its environment
  - an action is executed
  - agent receives the reinforcement signal or reward
    - signal can be positive or negative depending on the correctness of the action
- the exploration-exploitation trade-off is an important issue in RL
- RL agent components:
  - state: a description of the current situation in the environment
    - must contain enough information for the agent to solve its task, but does not have to be a complete description of the environment
  - policy: function used to specify which action to execute in each of the situations that the agent may encounter
  - reward: a quality measure of the environment's state
    - only an indirect measure of the agent’s state and actions
      - maybe incomplete
      - environment may be affected by other things as well
    - the goal of the agent is to maximize the total reward that it receives over the long run
  - action: affects the environment and hence, indirectly, the next state and reward
    - discrete or continuous (finite or infinite number of possible actions for each state)
  - value function: specifies the goal in the long run
    - finite-horizon model: agent optimizes its expected reward for the next ``n_t`` steps
    - infinite-horizon discounted model: take the entire long-run reward of the agent into consideration, however, each reward received in the future is geometrically discounted according to a discount factor ``[0, 1)``
- the policy is a function ``π(s)`` from states to actions
- the goal, then, is to find an optimal policy, ``π*(s)``, which for each state selects the action which maximizes the value function
- action selection should be stochastic
- the reward contains no direct information on which actions are good or bad, so the agent must explore, i.e. sometimes make actions which may seem sub-optimal at the time
  - example: epsilon-greedy
    - with probability ``ε``, explore by ignoring the suggestions from the learning system and select at random, otherwise, be greedy
    - to be be 'greedy' is to trust the learning system (to select the action that is currently believed to be the best)
- rewards should be associated with a sequence of actions (delayed RL), not only the latest one (immediate RL), since earlier decisions may also have contributed to the result
- temporal credit assignment problem: the core problem in delayed RL: How to decide which decisions in the action history to give credit/blame for the result
- supervised learning imitates, RL explores

### Model-Free Reinforcement Learning Model
- temporal difference (TD)
  - learns the value of the policy using the update rule
  - learns by sampling the environment according to some policy and it approximates its current estimate based on previously learned estimates (a process known as bootstrapping)
- SARSA
  - the Q-value, ``Q(s, a)`` is the estimated value of doing action ``a`` in state ``s``.
- Q-learning
  - the Q-value is defined under an assumption: ``Q(s, a)`` is the estimated value of doing action ``a`` in state ``s``, assuming that all future actions are greedy.

### Neural networks and Reinforcement Learning
- different approaches can be applied
  - use the NN to approximate the value function used to predict the future reward
  - use RL to adjust the weights of the NN
- gradient descent RL
  - can be used for problems where only the immediate reward is maximized (there's no value function, only a reward function)
  - performs a gradient descend on the expected reward
