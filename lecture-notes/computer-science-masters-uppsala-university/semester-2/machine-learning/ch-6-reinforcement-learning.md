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
  - policy: function used to specify which action to execute in each of the situations that the agent may encounter
  - reward: defines the goal of the agent
    - the reward is immediate an represents only the current environment state
    - the goal of the agent is to maximize the total reward that it receives over the long run
  - value function: specifies the goal in the long run
    - finite-horizon model: agent optimizes its expected reward for the next ``n_t`` steps
    - infinite-horizon discounted model: take the entire long-run reward of the agent into consideration, however, each reward received in the future is geometrically discounted according to a discount factor ``[0, 1)``
    - average reward model: prefers actions that optimize the agent's long-run average reward
      - with this model, it's not possible to distinguish between a policy that gains a large amount of reward in the initial phases, and a policy where the largest gain is obtained in the later phases

### Model-Free Reinforcement Learning Model
- temporal difference (TD)
  - learns the value of the policy using the update rule
  - learns by sampling the environment according to some policy and it approximates its current estimate based on previously learned estimates (a process known as bootstrapping)
- Q-learning
  - works by learning an action-value function which ultimately gives the expected utility of taking a given action ``a`` in a given state ``s``, and following an optimal policy thereafter

### Neural networks and Reinforcement Learning
- different approaches can be applied
  - use the NN to approximate the value function used to predict the future reward
  - use RL to adjust the weights of the NN
- resilient propagation (RPROP)
  - note: RPROP is a supervised learning algorithm, a modification to Backpropagation, and belongs in section 3.2
  - RPROP performs a direct adaptation of the weight step using local gradient information
    - if the weight of the partial derivative changes its sign, the weight update is decreased by a factor (this penalty is applied because the last weight update was too large, causing the algorithm to jump over a local minima)
    - if the derivative retains its sign, the update value is increase by a factor to accelerate convergence
- gradient descent RL
  - can be used for problems where only the immediate reward is maximized (there's no value function, only a reward function)
  - performs a gradient descend on the expected reward
