# Logical Reasoning Systems

## 10.1 Introduction

### Frame systems
**Frame systems** use the metaphor that objects are nodes in a graph, that these nodes are organized in a taxonomic structure, and that links between nodes represent binary relations. In frame systems the binary relations are thought of as slots in one frame that are filled by another. 

### Semantic networks
Just like **frame systems**, **semantic networks** use the metaphor that objects are nodes in a graph, that these nodes are organized in a taxonomic structure, and that links between nodes represent binary relations. In semantic networks, however, the binary relations are thought of as arrows between nodes. In general, though, the term **semantics network** is used to describe both **semantic networks** and **frame systems**. Semantic networks differ in how they handle the case of inheriting multiple different values.

### Procedural attachment
Semantic networks are very limited in their expressiveness. For example, it is not possible to represent negation (Opus does not ride a bicycle), disjunction (Opus appears in either the Times or the Dispatch), or quantification (all of Opus' friends are cartoon characters). These constructs are essential in many domains. A common approach is to use **procedural attachment**. It retains the limitations on expressiveness and uses procedural attachment to fill in the gaps. Procedural attachment is a technique where a function written in a programming language can be stored as the value of some relation, and used to answer ASK calls about that relation (and sometimes TELL calls as well).

## 10.2 Indexing, Retrieval and Unification

### Precision
**Precision** (also called **positive predictive value**) is the fraction of relevant instances among the retrieved instances.

### Recall
**Recall** (also known as sensitivity) is the fraction of relevant instances that have been retrieved over the total amount of relevant instances.

### F1-score
The **F1-score** (also called F-score or F-measure) is a measure of a test's accuracy. It considers both the **precision** and the **recall** of the test to compute the score.

<sup>(2 * precision * recall)</sup>/<sub>(precision + recall)</sub>