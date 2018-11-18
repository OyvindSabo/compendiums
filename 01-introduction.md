# Introduction

## Why Parallel Computing?

### 1.3 Why we need to write parallel programs

*"An efficient parallel implementation of a serial program may not be obtained by
finding efficient parallelizations of each of its steps. Rather, the best parallelization may be obtained by stepping back and devising an entirely new algorithm."* - Peter Pacheco

## 2.1 Some Background

### 2.1.1 The von Neumann architecture
*"The separation of memory and CPU is often called the **von Neumann bottleneck**, since the interconnect determines the rate at which instructions and data can be accessed."* - Peter Pacheco

## 2.6 Performance

### 2.6.1 Speedup and efficiency

**Linear speedup**\
The best we can hope to do is to equally divide the work among the cores,
without introducing additional work for the cores.

If we accomplish this and run the program with **p** cores, with one thread or process on each core, the parallel program will run **p** times faster than the serial program.

If we call the serial run-time **T<sub>serial</sub>** and our parallel run-time **T<sub>parallel</sub>**, then the best we can hope for is **T<sub>parallel</sub> = T<sub>serial</sub>/p**. When this happens, we say that our parallel program
has **linear speedup**.

**Superlinear speedup**\
A parallel program that obtains a speedup greater than **p** (the number of processes or threads) is said to have superlinear speedup.

For instance, a program that must use secondary storage for its data when it’s run on a single processor system might be able to fit all of its data into main memory when run on a large distributed-memory system.

Theoretical spuperlinear speedup is a contradiction because speedup, by definition is computed with respect to the best sequential algorithm.

**Efficiency**\
If S is the Speedup and P is the number of processes, the efficiency is given by **E = S/P**

**Parallel overhead**\
In general, dividing the work of a serial program among the processes/threads introduces a “parallel overhead” such as mutual exclusion or communication. If **T<sub>overhead</sub>** denotes this parallel overhead, it’s often the case that **T<sub>parallel</sub> = T<sub>serial</sub>/p + T<sub>overhead</sub>**.

### 2.6.2 Amdahl's law

**Amdahls law**\
If a fraction **r** of our serial program remains unparallelized, then Amdahl’s law says we can’t get a speedup better than **1/r**.

**Gustafson's law**\
Amdahl's law applies only to cases where the problem size is fixed. Gustafson argues that as more computing resources become available, they tend to get used on larger datasets, and the time spent in the parallelizable part often grows much faster than the time spent in parts which have to be executed in serial. This means that as the number of processes increase, the speedup achieved by parallelization might actually exceed the maximum speedup calculated by Amdahls law, since the increased data size means that the fraction of the program which has to be executed in serial has decreased for this larger data set. Therefore, Gustafson's law gives a less pessimistic and more realistic assessment of the parallel performance.