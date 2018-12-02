# Planning

## 11.4 Basic Representations for Planning

### The STRIPS language
**STRIPS** (**St**andford **R**esearch **I**nstitute **P**roblem **S**olver) is a State-based view of time. Actions are external to the logic. Given a state and an action, the STRIPS representation is used to determine whether the action can be carried out in the state and what is true in the resulting state.

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