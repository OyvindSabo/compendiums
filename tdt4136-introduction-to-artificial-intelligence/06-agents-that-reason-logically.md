# Agents that Reason Logically

# 6.3 Representation, Reasoning and Logic

### Closed-world assumption
The **closed-world assumption**, in a formal system of logic, is the presumption that a statement that is true is also known to be true. Therefore, conversely, what is not currently known to be true, is false.


### Entailment
An sentece α entails a sentence β if and only if there is no truth assignment that makes α true and β false. If one sentence entails another it menas that the other sentence is logically deductible from the first one.

**Example**\
A ∧ B ∧ C ⊨ B

### Circumscription
Circumscription allows the entailed sentences to be removed after new sentences added to the knowledge base.

### Inference
The terms **"reasoning"** and **"inference"** are generally used to cover any process by which conclusions are reached.

### Validity
A sentence is **valid** or **necessarily true** if and only if it is true under all possible interpretations in all possible worlds, that is, regardless of what it is supposed to mean and regardless of the state
of affairs in the universe being described.

**Example**\
Let's consider whether the following sentence is valid.\
∀x[[Student(x) ∧ ¬Student(x)] ⇒ BornOn(x, Moon)]

TO take up less space, let's define the following:\
S(x) = Student(x)\
B(x, y) = BornOn(x, y)

Let's solve this by using a truth table:
| S(x)  | ¬S(x) | S(x) ∧ ¬S(x) | B(x, Moon) | [S(x) ∧ ¬S(x)] ⇒ B(x, Moon) |
|-------|-------|--------------|------------|------------------------------|
| True  | False | False        | False      | True                         |
| False | True  | False        | False      | True                         |

The sentence is true for all S(x), and therefore all x. This means that the sentence is valid.

### Satisfiability
A sentence is satisfiable if and only if there is some interpretation in some world for which it is true.

### Unsatisfiability
A sentence that is not satisfiable is unsatisfiable. Self-contradictory sentences are unsatisfiable, if
the contradictoriness does not depend on the meanings of the symbols.

## 6.4 Propositional Logic: A Very Simple Logic
A sentence such as (P ∧ Q) ⇒ R is called an **implication** (or **conditional**). Its **premise** or **antecedent** is P A Q, and its **conclusion** or **consequent** is R.

### What's great about propositional logic?
- Propositional logic is declarative: Pieces of syntax correspond to facts
- Propositional logic allows partial/disjunctive/negated information
- Propositional logic is compositional

### What's bad about propositional logic?
- Propositional logic has very limited expressive power.

### Horn sentences
A **Horn sentence** (also called **horn clause**) is a disjunction of literals with at most one positive, i.e. unnegated, literal.

A Horn sentence has the form: PI ∧ P2 ∧ ... ∧ Pn ⇒ Q

Not every knowledge base can be written as a collection of Horn sentences