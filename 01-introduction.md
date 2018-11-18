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
The best we can hope to do is to equally divide the work among the cores,
without introducing additional work for the cores.

If we accomplish this and run the program with p cores, with one thread or process on each core, the parallel program will run p times faster than the serial program.

If we call the serial run-time **T<sub>serial</sub>** and our parallel run-time **T<sub>parallel</sub>**, then the best we can hope for is **T<sub>parallel</sub> = T<sub>serial</sub>/p**. When this happens, we say that our parallel program
has **linear speedup**.

In general, dividing the work of a serial program among the processes/threads introduces a “parallel overhead” such as mutual exclusion or communication. If **T<sub>overhead</sub>** denotes this parallel overhead, it’s often the case that **T<sub>parallel</sub> = T<sub>serial</sub>/p + T<sub>overhead</sub>**.