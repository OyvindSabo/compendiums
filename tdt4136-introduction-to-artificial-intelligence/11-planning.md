# Planning

## 11.4 Basic Representations for Planning

### The STRIPS language
The "classical" approach that most planners use today describes states and operators in a restricted
language known as the **STRIPS** (**St**andford **R**esearch **I**nstitute **P**roblem **S**olver) language  or in extensions thereof. The STRIPS language lends itself to efficient planning algorithms, while retaining much of the expressiveness of situation calculus representations.

STRIPS operators consist of three components:
- **The action description** is what an agent actually returns to the environment in order to do something. Within the planner it serves only as a name for a possible action.
- **The precondition** is a conjunction of atoms (positive literals) that says what must be true before the operator can be applied.
- **The effect** of an operator is a conjunction of literals (positive or negative) that describes how the situation changes when the operator is applied.

**Example of a STRIPS operator**\
Op(ACTION:Go(there), PRECOND:At(here)A Path(here,there), EFFECT:At(there)A ¬At(here))