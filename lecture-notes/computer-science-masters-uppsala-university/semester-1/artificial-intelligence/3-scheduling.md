# Lecture: Scheduling

Readings: Ch. 4 & 5, Russell, Stuart Jonathan; Norvig, Peter Artificial Intelligence: A Modern Approach 3

### Scheduling
- when to do required actions (given required resources)
- the task of scheduling is to provide an optimal schedule for the performance of the required actions. Optimal may be in terms of time (generally shortest) or resources (generally minimal use) or both

### Resource-Free Scheduling
- dynamic programming for resource-free scheduling
  - assumes we have a partial ordering of the actions required to achieve a goal
  - set ``ES(Start) = 0``
  - compute ``ES`` = Earliest start
    - ``ES(B) = ES(A) + Duration(A)`` (take max if there are competing nodes)
  - set ``LS(Finish) = ES(Finish)``
  - compute ``LS`` = Latest start
    - ``LS(A) = LS(B) - Duration(A)`` (take min if there are competing nodes))
  - finally, the critical path is the path from origin to goal where all nodes have ``slack - 0``
  - [Good explanation on how to do this](https://www.youtube.com/watch?v=4oDLMs11Exs)

### Priority Based Scheduling With Resource Constraints
- minimum slack algorithm as heuristic to order the queue
  - uses a priority queue based on the slack of each action
  - no always optimal
  - no scalable optimal algorithm exists
