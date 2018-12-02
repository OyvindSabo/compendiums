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

**Example**\
Consider the payoff matrix below. Wach agent can choose to go either north or south, for instance as part of a war strategy.

| agent i \ agent j | North | South |
|-------------------|-------|-------|
| **North**         | 2, -2 | 2, -2 |
| **South**         | 1, -1 | 3, -3 |
Let's find out which options will give the weakly dominant equilibrium.

Let's first consider the actions from agent i's perspective.
- If agent j goes North, then agent i's dominant action is to go North.
- If agent j goes South, then agent i's dominant action is to go South.

This means that agent i has no dominant strategy.

Next, let's consider the actions from agent j's perspective.
- If agent i goes North, then agent j's dominant action is to go either North or South.
- If agent i goes South, then agent j's dominant action is to go North.

This means that going north is a weakly dominant action for agent j, since it's always better or as good as any other action.

We can then assume that agent j will choose to go North. This means that agent i no longer has to consider the option of agent j going South. Agent i then only has to consider the following paayoff matrix:
| agent i \ agent j | North |
|-------------------|-------|
| **North**         | 2, -2 |
| **South**         | 1, -1 |

When assuming that agent j is going North, the dominant action for agent i is to go North as well.

This means that the weakly dominant equilibrium is for both agents to go North.

### Nash Equilibrium
A Nash equilibrium, named after John Nash, is a set of strategies, one for each player, such that no player has incentive to unilaterally change her action. Players are in equilibrium if a change in strategies by any one of them would lead that player to earn less than if she remained with her current strategy. For games in which players randomize (mixed strategies), the expected or average payoff must be at least as large as that obtainable by any other strategy.