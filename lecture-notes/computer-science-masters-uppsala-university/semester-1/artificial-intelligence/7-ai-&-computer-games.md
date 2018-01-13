# Lecture: AI & Computer Games - Guest Lecture

### AI in Computer Games
- goals
  - games are for entertainment, and thus it’s crucial that things behave naturally, which might include non-perfect behavior. It is also necessary to note that things represented in a game are not always creatures. Finally, characters of the game need to be aware, follow the natural laws (of the game), and avoid cheating.
  - game AI (or is it AL - Artificial life?)
    - academia is usually concerned with making rational decisions, such as searching for the optimal solution of a problem. In the other hand, game AI is more often about artificial life, believable behavior (which includes stupidity, realistic physics), and game balancing (challenging, but not unbeatable opponents).

### History
- 1960
  - first computer games
  - examples
    - SpaceWar! (PDP-1, for two human players)
    - board games (e.g. chess) against the machine
- 1970
  - computer controlled opponents
  - predefined patterns, not awareness
  - 'AI' takes 1-2% of CPU
  - examples
    - Pong
    - Space Invaders
- 1980
  - aware opponents with personality
  - first fighting games
  - first MORPG (multi-player online role playing games)
  - a computer beats a master chess player (1983)
  - examples
    - Pac-Man
    - adventure games
      - Dungeon, Zork, ...
    - (MUD - Multi-User Dungeon)
- 1990
  - FPS (first person shooter) and RTS (real-time strategy) games
  - games about/with evolution and learning
  - Deep Blue beats Kasparov (1997)
  - graphic cards take the load off the CPU
  - AI takes 10-35% of CPU
  - examples
    - (Creatures, and (2001) Black&White) —> evolution and learning
- 2000
  - focus shift from single to multi-player
  - focus shift from graphics to AI (and physics)
  - large part of the code is AI code (often made from scratch for each game)
  - less cheating
  - characters are more aware (thanks to better physics engines)
  - characters collaborate better
  - game industry in Sweden is big

### Typical Game AI topics
- strategical/tactical decisions
  - against or with you
  - search for best counter action
  - adaptivity
- director level AI
- simulation (natural behavior, animation)
- shortest path problems

### Common issues and methods
- why is Game AI hard? —> What makes it interesting to computer scientists?
  - huge state space
  - huge action space
  - multiple tasks (on different levels of abstraction, of different types)
  - non-deterministic (post-conditions difficult to set, makes planning difficult)
  - often real time
- common methods
  - Minimax (logic games, search for best counter action)
  - FSM - Finite state machines (define behavior)
  - A* search (for shortest path problems - notice best path is now always the shortest)
  - particle methods (simulation of flocks, smoke, water, grass, ...)
  - smart terrain (store knowledge in objects instead of in the characters)
    - easier to know what is relevant
    - easier to add new objects later
- machine learning
  - game characters are short lived, and learning requires many attempts
  - probabilistic methods (director level AI)
  - evolutionary methods (genetic algorithms and particle swarm optimization)
  - neural networks (in game development, but not often in the game)


### Issues with various game categories
- board games
  - discrete time/turn based and often deterministic
  - AI is the opponent and its goal is non-typical (for games) as it strives for optimality
  - tree search or reinforcement learning
- role playing games
  - AI in enemies, bosses, etc
  - FSM, grammar machines, quest generators
  - town behavior
    - need-based system
    - problems:
      - finding people
      - unwanted interaction between NPCs
- strategy games
  - shortest path problems, strategical decisions, tactical decisions
  - town building and resource management
- action games
  - cooperative agents, path finding, spatial reasoning
- racing games
  - track AI, traffic, physics, tuning vehicle parameters
- platform and sports games
  - camera problems, cooperation, learning

### Summary
- games require more than good graphics
- computer controlled characters must behave naturally, be reasonable intelligent, and not cheat
- graphics have dedicated hardware, which leaves more processing power available to AI
- in the future
  - dedicated AI cards?
  - combined AI/Physics/Graphics cards?
  - dedicated cores?
  - from simulated to real worlds (robotics)
