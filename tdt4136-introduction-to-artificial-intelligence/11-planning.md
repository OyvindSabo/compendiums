# Planning

## 11.4 Basic Representations for Planning

### The STRIPS language
**STRIPS** (**St**andford **R**esearch **I**nstitute **P**roblem **S**olver) is a State-based view of time. Actions are external to the logic. Given a state and an action, the STRIPS representation is used to determine whether the action can be carried out in the state and what is true in the resulting state.

**STRIPS representation of an operation**
- **The action:** What an agent actually returns to the environment in order to do something. Within the plariner it serves only as a name for a possible action.
- **The precondition:** A conjunction of atoms (positive literals) that says what must be true before the operator can be applied.
- **The effect:** A conjunction of literals (positive or negative) that describes how the situation changes when the operator is applied.

**STRIPS representation of an action**\
The STRIPS representation for an action consists of:
- **Preconditions:** A list of atoms that need to be true for the action to occur.
- **Delete list:** A list of those primitive relations no longer true after the action.
- **Add list:** A list of the primitive relations made true by the action

**Example of withdrawing cash from an ATM in STRIPS**
```
Action (withdraw(cash),
  PRECOND: At(ATM) ∧ Sells(ATM, cash, person) ∧ hasMoneyOnAccount(person)
  DELETE-LIST: hasMoneyOnAccount(person)
  ADD-LIST: have (cash))
```

### PDDL
**PDDL** (**P**roblem **D**omain **D**escription **L**anguage) is a recent attempt to standardize planning domain and problem description languages. It was developed mainly to make the 1998/2000 International Planning Competitions possible. **PDDL** contains **STRIPS**, **ADL** and more.

**Example of withdrawing cash from an ATM in PDDL**
```
Action (withdraw(cash),
  PRECOND: At(ATM) ∧ Sells(ATM, cash, person) ∧ hasMoneyOnAccount (person)
  EFFECT: -hasMoneyOnAccount (person) ∧ have (cash) )
```

### Representations for plans

**Least commitment**\
Many planners use the principle of **least commitment**, which says that one should only make choices about things that you currently care about, leaving the other choices to be worked out later. This is a good idea for programs that search, because if you make a choice about something you don't care about now, you are likely to make the wrong choice and have to backtrack later.

**Partial order planner**\
A planner that can represent plans in which some steps are ordered (before or after) with respect to each other and other steps are unordered is called a **partial order planner**. It only commits ordering between actions when forced to.

A partial order plan consists of four components:
- A set of **actions** (also known as operators).
- A **partial order** for the actions. It specifies the conditions about the order of some actions.
- A set of **causal links**. It specifies which actions meet which preconditions of other actions. Alternatively, a set of bindings between the variables in actions.
-  set of **open preconditions**. It specifies which preconditions are not fulfilled by any action in the partial-order plan.

**Total order planner**\
A **total order planner** is a planner whose plan is a simple list of steps.

**Linearization**\
A totally ordered plan that is derived from a plan P by adding ordering constraints is called a
**linearization** of P.