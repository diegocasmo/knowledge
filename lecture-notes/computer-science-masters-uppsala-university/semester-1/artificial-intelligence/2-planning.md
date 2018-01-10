# Lecture: Planning

Readings: Ch. 4 & 5, Russell, Stuart Jonathan; Norvig, Peter Artificial Intelligence: A Modern Approach 3

### Planning
- PDDL: planning domain definition language
  - states
    - specified by a set of atomic 'fluents' (statements that can be true or false)
    - atomic means that they do not include conjunctions, dis-junctions, conditionals, or negations
    - database semantics are assumed, allowing negation by omission
  - actions
    - specified by a set of preconditions and effects
    - both 'preconditions' and 'effects' are conjunctions of 'literals' - atomic propositions or negations of atomic propositions. all preconditions are current, all effects immediate
    - actions can be performed at a state where all positive literals are, and all negative literals are not in the set of fluents at a state
    - the effect of an actions is to add all positive literals, and remove all negative literals, in the action's effects from the current state
  - goals
    - specified by a conjunction of literals
    - we have achieved a goal state when all positive literals are, and all negative literals are not included in the current state specification
- solving PDDL problems with planning graphs
  - a planning graph is an approximation of how many steps it would take to reach a goal
    - estimate is always correct when it reports the goal is unreachable
    - it never overestimates the number of steps (it's admissible)
  - directed graphs, organized into alternating state and action levels
    - ``s_0, a_0, s_1, a_1, ..., a_{n - 1}, s_n``
  - each action at ``a_i`` its connected to its precondition at ``s_i`` and effects at ``s_{i + 1}``
  - trivial 'no-op' actions are given for all literals. No-op action for literal ``P``:
    - preconditions: ``P``
    - effects: ``P``
- tracks (some) mutual exclusion relations
  - actions
    - one action's effects negates an effect of the other
    - one action's effect is the negation of a precondition of the other
    - a precondition for one action is marked as mutually exclusive to a precondition for the other. in other words, both actions need the same mutex pre-condition (notice the action might also be competing with a no-op)
  - states
    - one is the negation of the other
    - if each possible pair of actions that could produce the literals are mutually exclusive
- what use are planning graphs?
  - if any goal literal fails to appear in the final state level, then the problem is unsolvable
  - the max-level heuristic given the number of state levels until all goal literals are in the graph
  - the set-level heuristic given the number of state levels until all goals are in the graph and there are no mutex relations between any pair of goal literals
  - we can obtain a plan directly of the planning graph using the graph planning algorithm, but this is generally less efficient
