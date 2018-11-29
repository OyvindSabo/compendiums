# Game Playing

## 5.2 Perfect Decisions in Two-Person Games

**Example**\
In the following **adversarial** game it is MAX’s turn to play. The numbers at
each leaf node is the estimated score of that position. Check nodes from left to right order.
![minimax not filled.png](https://cdn.steemitimages.com/DQmPk5YQDLH1MR8p5nSC3N1M3HXWoTWeMaZFXvmgu7gqPpo/minimax%20not%20filled.png)
Perform mini-max search and label each branch node with its value.

The Max player will always try to maximize the score, and the Min player will always try to minimize the score. Note that the score is the utility evaluation of the maximizing player's state, even when it's the minimizing player's turn.
![minimax filled.png](https://cdn.steemitimages.com/DQmbA7fvZduvjiXrfKWDYg4LKAqJPgrjr7j18AHbSxjTdWA/minimax%20filled.png)
This is the mini-max search tree with the branch nodes labeled with their values. We see that Max's best move is B, which will give a utility of 5.