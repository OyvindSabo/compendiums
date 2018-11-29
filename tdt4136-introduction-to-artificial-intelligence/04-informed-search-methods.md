# Informed Search Methods

## 4.2 Heuristic Functions

### Most-constrained-variable heuristic
Most-constrained-variable heuristic is used with **forward checking**, which keeps track of which values are still allowed for each variable, given the choices made so far. At each point in the search, the variable with the fewest possible values is chosen to have a value assigned. In this way, the branching factor in the search tends to be minimized.