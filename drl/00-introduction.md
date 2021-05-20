# Reinforcement Learning
- How to map situations to actions to maximize a numerical reward signal.
- Learner is not told what actions to take, instead must discover which actions yield the most reward by trying them.
- Two main characteristics: Trial and error and Delayed reward
- A learning agent must be able to sense the state of its environment and, to some extent, take actions that affect that state
- Markov decision process includes three aspects: sensation, action, and goal.
- In uncharted territory - an agent must learn from its own experience
An RL agent has to *exploit* what it has already experienced to obtain the reward, but it also has to *explore* to make a better action selection in the future. There is an issue of balancing *exploration-exploitation*.
In contrast to RL, evolutionary methods apply multiple static policies, and each interacting over a long period of time with a separate instance of the environment. Evolutionary methods are useful when the probability of finding a good action is high.

## Elements of Reinforcement Learning
- *Policy:* Learning agent's way of behaving at a given time. Mapping from perceived states of the environment to actions that can be taken in those states. The policy may be stochastic, specifying the probability for each action. 
- *Reward signal:* Goal of the RL problem. An agent's objective is to maximize the reward it receives over the long run. 
- A reward specifies what is good in an immediate sense, a *value function* specifies what is good in the long run. A value of a state is the total amount of reward an agent can expect to accumulate over the future, starting from that state. 
- We seek actions that yield the highest value, not the highest reward, as some actions give the highest reward over the long run.
- *Model:* mimics the behavior of the environment. It might predict the next state and next reward. Model is used for planning any way of deciding a course of action by considering possible future situations before they are experienced. This is called a *model-based* approach instead of the much simpler *model-free* approach.

## Tic-Tac-Toe
Classical "minimax" solution never reaches a game state from which it could lose, even if, in fact, it always won from that state because of incorrect play by the opponent. 

The evolutionary method holds the policy fixed and plays many games against the opponent or simulates many games using a model of the opponent. The frequency of wins gives an unbiased estimate of the probability with that policy, can be used directly for the following policy selection. But each policy change is made only after several games, and what happens in the game is ignored. 

Value function allows individual states to be evaluated. Learning a value function takes advantage of the information available during play.

DRL provides the ability for the program to generalize from its experience. How well an RL system can work in problems with such large state sets is intimately tied to how appropriate it can generalize from past experience. 