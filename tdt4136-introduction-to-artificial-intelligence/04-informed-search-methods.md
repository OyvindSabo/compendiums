# Informed Search Methods



## 4.2 Heuristic Functions

### Most-constrained-variable heuristic
Most-constrained-variable heuristic is also called **minimum-remaining-values heuristic**. It is used with **forward checking**, which keeps track of which values are still allowed for each variable, given the choices made so far. At each point in the search, the variable with the fewest possible values is chosen to have a value assigned. In this way, the branching factor in the search tends to be minimized.

**Example**\
Given this table of partially assigned colors for the nodes in a graph, list all unassigned variables which variables might be selected by **minimum-remaining-values heuristic (MRV)**.
| A   | B     | C   | D     | E   | F     |
|-----|-------|-----|-------|-----|-------|
| R B | Green | R B | R G B | R B | R G B |

**MRV** will consider the variables with the fewest possible values. In this case, the nodes are the variables and their color is their value. The unassigned variables with the fewest possible values are **A, C and E**, so those are the variabes which might be selected by **MRV**.


### Most-constraining-variable heuristic
**Most-constraining-variable heuristic** is also called **degree heuristic**.
The most-constraining-variable heuristic is similarly effective. It attempts to reduce the branching factor on future choices by assigning a value to the variable that is involved in the largest number of constraints on other unassigned variables.

**Example**\
Let's say we have the following undirected graph:
```
V = { A, B, C, D, E, F }
E = {
  { A, B },
  { A, E },
  { B, C },
  { B, E },
  { C, D },
  { C, E },
  { D, F },
  { E; F }
}
```
Assume we want to color the nodes. Each node node must be colored one of Red (R), Green (G) or Blue (B). Adjacent nodes must be a different color. Given this table of partially assigned colors for the nodes in a graph, list all unassigned variables that might be selected by the **degree heuristic**.
| A   | B     | C   | D     | E   | F     |
|-----|-------|-----|-------|-----|-------|
| R B | Green | R B | R G B | R B | R G B |
Unassigned variables depending on **A** = **E**\
Unassigned variables depending on **C** = **D, E**\
Unassigned variables depending on **D** = **C, F**\
Unassigned variables depending on **E** = **A, C, D, F**\
Unassigned variables depending on **F** = **D, E**

We see that E is the variable which is involved in the largest number of constraints on other unassigned variables.

Since 

### Least-constraining-value heuristic
Once a variable has been selected, we still need to choose a value for it. One's intuition is that the value which leaves more freedom for future choices, is the better value. This intuition is the **least-constraining-value heuristic** — choose a value that rules out the smallest number of values in variables connected to the current variable by constraints.

## 4.4 Iterative Improvement Algorithms

### Min-conflicts heuristics
In choosing a new value for a variable, the most obvious heuristic is to select the value that results in the minimum number of conflicts with other variables — **the min-conflicts heuristic**.

**Example**\
In a graph in which each neighbouring node cannot have the same color, a single node can be either **Red**, **Green** or **Blue**. Consider the complete but inconsistent assignment of colors in the table below. **E** has been selected to be assigned a value during local search for a complete and consistent assignment. What new value would be chosen for **E** by the **minimum-conflict heuristic?**
| A    | B     | C     | D     | E | F     |
|------|-------|-------|-------|---|-------|
| Blue | Green | Green | Green | ? | Green |
**Red** is the color which conflicts least with the other selected colors. Infact it does not conflict at all. Therefore, **Red** is the value which will be chosen by the **minimum-conflict heuristic**.