# Inference in First-Order Logic

## 9.6 Resolution: A Complete Inference Procedure

### Refutation
**Refutation**, also known as **proof by contradiction** and **reductio ad absurdum** is idea is that to prove P, we assume P is false (i.e., add ¬P to the knowledge base) and prove a contradiction. If we can do this, then it must be that the knowledge base implies P.

**Example**\
Assume the following knowledge base KB:
```
∀ x allergies(x) ⇒ sneeze(x)
∀ x ∀ y cat(y) ∧ allergicToCats(x) ⇒ allergies(x)
cat(Felix)
allergicToCats(Mary)
```
The Goal/Query: Does Mary sneeze?, I.e. sneeze(Mary)?

Let's perform **resolution refutation** and find out if the query has a positive (True) or negative (False) answer for this knowledge base.

We add ¬sneeze(Mary) to the knowledge base, and check if it is contradictory.
```
∀ x allergies(x) ⇒ sneeze(x)
∀ x ∀ y cat(y) ∧ allergicToCats(x) ⇒ allergies(x)
cat(Felix)
allergicToCats(Mary)
¬sneeze(Mary)
```
| Premise                                                                              | Conclusion |
|--------------------------------------------------------------------------------------|-----------------|
| cat(Felix), allergicToCats(Mary), ∀ x ∀ y cat(y) ∧ allergicToCats(x) ⇒ allergies(x) | allergies(Mary) |
| allergies(Mary), ∀ x allergies(x) ⇒ sneeze(x)                                       | sneeze(Mary)    |
| sneeze(mary), ¬sneeze(Mary)                                                          | CONTRADICTION   |
Assuming that Mary does not sneeze results in a contradiction. This means that the answer to "Does Mary sneeze?" is **Yes**.