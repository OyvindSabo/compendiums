# Game Playing

## 5.2 Perfect Decisions in Two-Person Games

### Utility function
A **utility function** is also called a **payoff function**. It gives a numeric value for the outcome of a game. In chess, the outcome is a win, loss, or draw, which we can represent by the values +1, —1, or 0. One might also try to approximate utility functions for a given state of a game.

### Minimax
Consider the general case of a game with two players. Let's call the players MAX and MIN. Max moves first, and then they take turns moving until the game is over.

The **minimax algorithm** is designed to determine the optimal strategy for MAX, and thus to decide what the best first move is. The algorithm consists of five steps:
- Generate the whole game tree, all the way down to the terminal states.
- Apply the utility function to each terminal state to get its value.
- Use the utility of the terminal states to determine the utility of the nodes one level higher up in the search tree.
- Continue backing up the values from the leaf nodes toward the root, one layer at a time.
- Eventually, the backed-up values reach the top of the tree; at that point, MAX chooses the move that leads to the highest value. This is called the mininiax decision, because it maximizes the utility under the assumption that the opponent will play perfectly to minimize it.

**Example**\
In the following **adversarial** game it is MAX’s turn to play. The numbers at
each leaf node is the estimated score of that position. Check nodes from left to right order.
![minimax not filled.png](https://cdn.steemitimages.com/DQmPk5YQDLH1MR8p5nSC3N1M3HXWoTWeMaZFXvmgu7gqPpo/minimax%20not%20filled.png)
Perform mini-max search and label each branch node with its value.

The Max player will always try to maximize the score, and the Min player will always try to minimize the score. Note that the score is the utility evaluation of the maximizing player's state, even when it's the minimizing player's turn.
![minimax filled.png](https://cdn.steemitimages.com/DQmbA7fvZduvjiXrfKWDYg4LKAqJPgrjr7j18AHbSxjTdWA/minimax%20filled.png)
This is the mini-max search tree with the branch nodes labeled with their values. We see that Max's best move is B, which will give a utility of 5.

### Cutting off search
The most straightforward approach to controlling the amount of search is to set a fixed depth limit, so that the cutoff test succeeds for all nodes at or below depth d. The depth is chosen so
that the amount of time used will not exceed what the rules of the game allow. A slightly more
robust approach is to apply **iterative deepening**. When time runs out, the program returns the move selected by the deepest completed search. When cutting off search, the evaluation function should only
be applied to positions that are quiescent, that is, unlikely to exhibit wild swings in value in the
near future.

## 5.4 Alpha-beta pruning

Fortunately, it is possible to compute the correct minimax decision without looking at every node in the search tree. The process of eliminating a branch of the search tree from consideration without examining it is called pruning the search tree. When applied to a standard minimax tree, it returns the same move as minimax would, but prunes away branches that cannot possibly influence the final decision.

The general principle is this. Consider a node n somewhere in the tree (see Figure 5.7), such that Player has a choice of moving to that node. If Player has a better choice ra either at the parent node of n, or at any choice point further up, then n will never be reached in actual play. So once we have found out enough about n (by examining some of its descendants) to reach this conclusion, we can prune it.

Alfa-beta pruning is best illustrated with an example.

**Example**\
Consider the following mini-max search tree.
![minimax filled.png](https://cdn.steemitimages.com/DQmbA7fvZduvjiXrfKWDYg4LKAqJPgrjr7j18AHbSxjTdWA/minimax%20filled.png)

Assume that nodes are checked left to right, what nodes would be pruned by the **alpha-beta algorithm**?
![minimax alpha beta pruned.png](https://cdn.steemitimages.com/DQmeQeev1FEn6vJyestxLLxhnYFavJJuew5Q9nJz9d8avkx/minimax%20alpha%20beta%20pruned.png)
When Min discovers that L3 gives a higher score than L1, there is no reason to explore L4.
When Min discovers that L7 gives a higher score than L1, there is no reason to explore L8.
When Min discovers that L14 gives a higher score than L12, there is no reason to explore L15 and L16.
When Max discovers that L17 gives a lower score than L10, there is no reason to exlore L18, L19, L20, L21, L22 and L23.