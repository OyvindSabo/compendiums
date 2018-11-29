# Informed Search Methods

## 4.2 Heuristic Functions

### Most-constrained-variable heuristic
Most-constrained-variable heuristic is also called **Minimum-remaining-values heuristic**. It is used with **forward checking**, which keeps track of which values are still allowed for each variable, given the choices made so far. At each point in the search, the variable with the fewest possible values is chosen to have a value assigned. In this way, the branching factor in the search tends to be minimized.

**Example**\
Given this table of possible colors for the nodes in a graph, list all unassigned variables which variables might be selected by **Minimum-remaining-values heuristic (MRV)**.
| A   | B     | C   | D     | E   | F     |
|-----|-------|-----|-------|-----|-------|
| R B | Green | R B | R G B | R B | R G B |

**MRV** will consider the variables with the fewest possible values. In this case, the nodes are the variables and their color is their value. The unassigned variables with the fewest possible values are **A, C and E**, so those are the variabes which might be selected by **MRV**.


### Most-constraining-variable heuristic
The most-constraining-variable heuristic is similarly effective. It attempts to reduce the branching factor on future choices by assigning a value to the variable that is involved in the largest number of constraints on other unassigned variables.

### Least-constraining-value heuristic
Once a variable has been selected, we still need to choose a value for it. One's intuition is that the value which leaves more freedom for future choices, is the better value. This intuition is the **least-constraining-value heuristic** — choose a value that rules out the smallest number of values in variables connected to the current variable by constraints.