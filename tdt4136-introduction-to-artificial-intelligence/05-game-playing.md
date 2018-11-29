# Game Playing

## 5.2 Perfect Decisions in Two-Person Games

### Mini-max

**Example**\
In the following **adversarial** game it is MAX’s turn to play. The numbers at
each leaf node is the estimated score of that position. Check nodes from left to right order.
![minimax not filled.png](https://cdn.steemitimages.com/DQmPk5YQDLH1MR8p5nSC3N1M3HXWoTWeMaZFXvmgu7gqPpo/minimax%20not%20filled.png)
Perform mini-max search and label each branch node with its value.

The Max player will always try to maximize the score, and the Min player will always try to minimize the score. Note that the score is the utility evaluation of the maximizing player's state, even when it's the minimizing player's turn.
![minimax filled.png](https://cdn.steemitimages.com/DQmbA7fvZduvjiXrfKWDYg4LKAqJPgrjr7j18AHbSxjTdWA/minimax%20filled.png)
This is the mini-max search tree with the branch nodes labeled with their values. We see that Max's best move is B, which will give a utility of 5.

### Alpha-beta pruning

**Example**\
Consider the following mini-max search tree.
![minimax filled.png](https://cdn.steemitimages.com/DQmbA7fvZduvjiXrfKWDYg4LKAqJPgrjr7j18AHbSxjTdWA/minimax%20filled.png)

Assume that nodes are checked left to right, what nodes would be pruned by the **alpha-beta algorithm**?
![minimax alpha beta pruned.png](https://cdn.steemitimages.com/DQmeQeev1FEn6vJyestxLLxhnYFavJJuew5Q9nJz9d8avkx/minimax%20alpha%20beta%20pruned.png)
When Min discovers that L3 gives a higher score than L1, there is no reason to explore L4.
When Min discovers that L7 gives a higher score than L1, there is no reason to explore L8.
When Min discovers that L14 gives a higher score than L12, there is no reason to explore L15 and L16.
When Max discovers that L17 gives a lower score than L10, there is no reason to exlore L18, L19, L20, L21, L22 and L23.