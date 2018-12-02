# An Introduction to Game Theory

## Sreategic Form Solution Concepts

### Dominant strategy
A dominant strategy for a player is an action that is optimal for this player no matter what his opponents do. Put differently, a player with a dominant action does not have to
worry about how his opponents will play the game; for any belief that he might have about the plans
of actions by others, playing a dominant action is optimal.

### Strictly dominant strategy
A **strictly dominant strategy**, or **strongly dominant strategy** is that strategy that always provides greater utility to a the player, no matter what the other player’s strategy is;

**Example**\
Consider the payoff matrix below. Each agent can choose between the actions {0, 100, 200, 300}. These actions could for instance be bids at an auction.
| agent i \ agent j | 0       | 100     | 200      | 300    |
|-------------------|---------|---------|----------|--------|
| **0**             | 300,0$  | 200,0$  | 0,100$   | 0,0$   |
| **100**           | 200,0$  | 200,0$  | 200,100$ | 0,300$ |
| **200**           | 0,0$    | 0,200$  | 0,0$     | 0,0$   |
| **300**           | -100,0$ | -100,0$ | -100,0$  | -100,0 |
Let's find which set of options will give the strictly dominant equilibrium in this game.

Let's first consider the actions from agent i's perspective.
- If agent j chooses 0, then agent i's dominant action is 0.
- If agent j chooses 100, then agent i's dominant action is 0 and 100.
- If agent j chooses 200, then agent i's dominant action is 100.
- If agent j chooses 300, then agent i's dominant action is 100.

We can see that there is no action agent i can take which is dominant for every action agent j can take. This means that there exists no strictly dominant equilibrium for this game, lol.


### Weakly dominant strategy
A weakly dominant strategy is that strategy that provides at least the same utility for all the other player’s strategies, and strictly greater for some strategy.

### Nash Equilibrium
A Nash equilibrium, named after John Nash, is a set of strategies, one for each player, such that no player has incentive to unilaterally change her action. Players are in equilibrium if a change in strategies by any one of them would lead that player to earn less than if she remained with her current strategy. For games in which players randomize (mixed strategies), the expected or average payoff must be at least as large as that obtainable by any other strategy.