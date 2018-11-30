# Informed Search Methods

## 4.1 Best-First Search

Estimated remaining cost to the total goal: h(n)\
Cost of the path so far: g(n)

### A* search
**A\* search** aims to find a path to the given goal node having the smallest cost (least distance travelled, shortest time, etc.). At each iteration of its main loop, **A\*** needs to determine which of its paths to extend. It does so based on the cost of the path and an estimate of the cost required to extend the path all the way to the goal. Specifically, A* selects the path that minimizes

**f(n)=g(n)+h(n) f(n)=g(n)+h(n)**

where **n** is the next node on the path, **g(n)** is the cost of the path from the start node to **n**, and **h(n)** is a heuristic function that estimates the cost of the cheapest path from n to the goal. **A\*** terminates when the path it chooses to extend is a path from start to goal or if there are no paths eligible to be extended.

**Example**\
Let's say we have the following directed graph:
![A star.png](https://cdn.steemitimages.com/DQmeXPb6Q96GzgfDKj2ymQUvpaR5rCTSBJUhcnC8P8cMQ8b/A%20star.png)

The numbers on the edges represent the cost of traveling between the nodes connected by the edge.

h(S)= 7\
h(A)= 6\
h(B)= 2\
h(C)= 1\
h(G)= 0

Let's find the shortest path from S to G.

| Node which is currently being expanded | Queue                    |
|----------------------------------------|--------------------------|
| S(7)                                   | A(1+6, S), B(2+2, S)     |
| B(4, S)                                | C(4+1, SB), A(1+6, S)    |
| C(5, SB)                               | G(4+3, SBC), A(1+6, S)   |
| A(7, S)                                | G(4+3, SBC), G(1+13, SA) |
| G(7, SBC)                              | GOAL                     |
Shortest path: S B C A G

## 4.2 Heuristic Functions

### Consistent heuristics
If we want to find the shortest solutions, we need a heuristic function that never overestimates the number of steps to the goal. A heuristic function is said to be **consistent**, or **monotone**, if its estimate is always less than or equal to the estimated distance from any neighboring vertex to the goal, plus the cost of reaching that neighbor.

In other words, for every node **N** and each successor **P** of **N**, the estimated cost of reaching the goal from **N** is no greater than the step cost of getting to **P** plus the estimated cost of reaching the goal from **P**. That is:

h(N) &le; c(N,P)+h(P)\
h(G)=0

**Example**\
Consider the graph below, as well as the following set of heuristic values:
![A star.png](https://cdn.steemitimages.com/DQmeXPb6Q96GzgfDKj2ymQUvpaR5rCTSBJUhcnC8P8cMQ8b/A%20star.png)
1. {h(S) = 7, h(A) = 6, h(B) = 2, h(C) = 1, h(G) = 0}
2. {h(S) = 7, h(A) = 6, h(B) = 2, h(C) = 2, h(G) = 0}
3. {h(S) = 7, h(A) = 6, h(B) = 4, h(C) = 2, h(G) = 0}
4. {h(S) = 7, h(A) = 4, h(B) = 4, h(C) = 4, h(G) = 0}

Let's see which of the heuristic values are consistent with the cost of the edges in the graph.

**Let's consider the first set of heuristics**\
h(s) = 7\
c(S, B) + h(B) = 2 + 2 = 4\
==> h(s) > c(S, B) + h(B)\
==> Not consistent

**Let's consider the second set of heuristics**\
For the same reason as the first set of heuristics:
==> Not consistent

**Let's consider the third set of heuristics**\
h(s) = 7\
c(S, B) + h(B) = 2 + 4 = 6\
==> h(s) > c(S, B) + h(B)\
==> Not consistent

**Let's consider the fourth set of heuristics**\
For the same reason as the third set of heuristics:
==> Not consistent

It turns out that none of the heuristics were consistent, lol.



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