# Intelligent Agents

## 2.2 How Agents Should Act

**Ideal rational agent**\
For each possible percept sequence, an ideal rational agent should do whatever action is expected to maximize its performance measure, on the basis of the evidence provided by the percept sequence and whatever built-in knowledge the agent has.

## 2.3 Structure of intelligent agents

### Simple reflect agents
**Simple reflex agents** are purely reactive agents. They act only on the basis of the current percept, ignoring the rest of the percept history. The agent function is based on the condition-action rule: if condition then action. **Simple reflex agents** only succeeds when the environment is **fully observable**. Infinite loops are often unavoidable for **simple reflex agents** operating in **partially observable environments**. The agent might be able to break out of the loop if it randomizes its actions.

## 2.4 Environments

### Properties of Environments

- **Accessible** vs. **inaccessible** This refers to the possibility for an agent to obtain complete and accurate information about the environment’s state. An environment is effectively accessible if the sensors detect all aspects that are relevant to the choice of action. If the state of a game is not fully accessible, it means that the game is not 100% a skill-game, since luck might play a vital role.
- **Deterministic** vs. **non-deterministic** If the next state of the environment is completely determined by the current state and the actions selected by the agents, then we say the environment is deterministic. In a deterministic environment any action has a single guaranteed effect, and no failure or uncertainty. On the contrary is a non-deterministic environment. In a non-deterministic environment, the same task performed twice may produce different results or may even fail completely.
- **Episodic** vs. **nonepisodic** In an episodic environment, each agent’s performance is the result of a series of independent tasks performed. There is no link between the agent’s performance and other different scenarios. In other words, if the environment is episodic, the agent decides which action is best to take, it will only consider the task at hand and doesn’t have to consider the effect it may have on future tasks. 
- **Static** vs. **dynamic** An environment is static if only the actions of an agent modify it. It is dynamic on the other hand if other processes are operating on it. An environment is semidynamic if it is static, but the agent's score might change while the agent is deliberating.
- **Discrete** vs **continuous** An environment is said to be discrete if there are a finite number of actions that can be performed within it.

**Example**\
Let's categorize Backgammon using the properties of environment.


In backgammon, the whole state of the current board is observable. The opponent does not have information which is hidden from the current player. Therefore, the state is **accessible**.

In backgammon, the moves a player is allowed to make are dependent on the throw of two dices. That means that for any given move, the state of the next move will not solely depend on the decision made in this move. Therefore, the game state is **non-deterministic**.

In backgammon, only the current state of the game has to be considered. It doesn't matter what actions lead to the current state. In other words, backgammon is **episodic**.

In backgammon, the state of the game cannot change while the agent is deliberating. This means that the backgammon environment is **static**.

In backgammon, there is a finite number of states, as well as a finite number of possible actions per state. This means that the environment is **discrete**.


in some competitive environments, randomized behavior is rational because
it avoids the pitfalls of predictability.