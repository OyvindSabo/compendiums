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

## 9.6 Conversion to Normal Form
- **Eliminate implications:** Replace all implications by the corresponding disjunctions.\
p ⇒ q becomes ¬p V q.
- **Move ¬ inwards:** Move negations inwards using de Morgan's law:\
¬(p V q) becomes ¬p A ¬q\
¬(p A q) becomes ¬p V ¬q\
¬∀ x,p becomes Ǝx ¬p\
¬Ǝ x,p becomes ∀x ¬p\
¬¬p becomes p
- **Distribute A over V:**  (a ∧ b) V c becomes (a V c) ∧ (b V c).
- **Flatten nested conjunctions and disjunctions:**\
(a V b) V c becomes (a V b V c)\
(a A b) ∧ c becomes (a ∧ b ∧ c)

Also note that ¬A V A can be remove, since it doesn't say anything.

**Example**\
Let's consider the sentence:\
"Men and women are welcome to apply."

We write this as first-order logic:\
∀x [(M(x) V W(x)) ⇒ Apply(x)]

First, let's eliminate implications:\
∀x [¬(M(x) V W(x)) V Apply(x)]

Next, we move negations inwards:\
∀x [(¬M(x) ∧ ¬W(x)) V Apply(x)]

Next, we distribute A over V:\
∀x [(¬M(x) V Apply(x)) ∧ (¬W(x) V Apply(x))]

Lastly, we flatten nested conjunctions and disjunctions. In this case there are no unflattened conjunctions nor disjunctions:\
∀x [(¬M(x) V Apply(x)) ∧ (¬W(x) V Apply(x))]

### Skolemization
Skolemization is the process of removing existential quantifiers (Ǝ) by elimination.\

Not skolemized: ∀x Person(x) ⇒ Ǝy Heart(y) ∧ Has(x,y)\
Skolemized: ∀x Person(x) ⇒ Heart(F(x)) ∧ Has(x,F(x))

Note that the reason we swap y with F(x), and not just a constant F, is that in this case for every x, there exists at least one y. It is not given that the y which exists is the same for every x. We therefore need to make y a function of x.

**Example**
Let's skolemize the following sentence:\
∀x[(¬P(x) ∧ Q(x)) ∨ ∃y(R(x, y) ∧ T(y)))]

We want to remove the existential quantifier for y, so we need to swap y with f(x).\
∀x[(¬P(x) ∧ Q(x)) ∨ (R(x, f(x)) ∧ T(f(x))))]

## 9.7 Completeness of Resolution

### Rewrite to clausal form
- Get rid of Ǝ.
- Rewrite A ⇒ B to ¬A V B