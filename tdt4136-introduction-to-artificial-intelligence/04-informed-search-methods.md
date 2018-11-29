# Informed Search Methods

## 4.2 Heuristic Functions

### Most-constrained-variable heuristic
Most-constrained-variable heuristic is used with **forward checking**, which keeps track of which values are still allowed for each variable, given the choices made so far. At each point in the search, the variable with the fewest possible values is chosen to have a value assigned. In this way, the branching factor in the search tends to be minimized.

### Most-constraining-variable heuristic
The most-constraining-variable heuristic is similarly effective. It attempts to reduce the branching factor on future choices by assigning a value to the variable that is involved in the largest number of constraints on other unassigned variables.

### Least-constraining-value heuristic
Once a variable has been selected, we still need to choose a value for it. One's intuition is that the value which leaves more freedom for future choices, is the better value. This intuition is the **least-constraining-value heuristic** — choose a value that rules out the smallest number of values in variables connected to the current variable by constraints.