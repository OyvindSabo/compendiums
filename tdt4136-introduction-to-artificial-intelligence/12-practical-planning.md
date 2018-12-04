# Practical Planning

### Under which conditions does a plan graph level off?
For a plan graph to level off (stops expanding), the last state must include all goals in the goal state without mutex between them, and when back-searching on the graph there must be actions without mutex at each action level so that this chaining goes back to the initial state. Or the two consequtive states are equal- no change any longer.
