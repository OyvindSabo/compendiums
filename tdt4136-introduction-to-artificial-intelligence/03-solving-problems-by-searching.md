### Arc-consistency
**What is arc-consistency**\
A state is **arc-consistent** if every variable has a value in its domain that is consistent with each of the constraints on that variable. Arc consistency can be achieved by successive deletion of values that are inconsistent with some constraint. As values are deleted, other values may become inconsistent because they relied on the deleted values.

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
Assume we want to color the nodes. Each node node must be colored one of Red (R), Green (G) or Blue (B). Adjacent nodes must be a different color.
| A    | B     | C     | D     | E    | F     |
|------|-------|-------|-------|------|-------|
| Blue | R G B | R G B | R G B | Red  | R G B |
Consider this partial assignment where variable A and E have been assigned values. Let's cross out all values that should be eliminated by Arc Consistency.

First, we elliminate Blue (B) from all nodes connected to A:
| A    | B   | C     | D     | E   | F     |
|------|-----|-------|-------|-----|-------|
| Blue | R G | R G B | R G B | Red | R G B |

Next, we elliminate Red (R) from all nodes connected to E:
| A    | B     | C   | D     | E   | F   |
|------|-------|-----|-------|-----|-----|
| Blue | Green | G B | R G B | Red | G B |

Now that B is decided, we can remove Green (G) from all nodes connected to B:
| A    | B     | C     | D     | E    | F   |
|------|-------|-------|-------|------|-----|
| Blue | Green | Blue  | R G B | Red  | G B |

Now that C is decided, we can remove Blue (B) from all nodes connected to C:
| A    | B     | C    | D   | E   | F   |
|------|-------|------|-----|-----|-----|
| Blue | Green | Blue | R G | Red | G B |

This is as far as Arc-consistency will take us.