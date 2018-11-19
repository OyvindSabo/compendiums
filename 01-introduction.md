# Introduction

## Why Parallel Computing?

### 1.3 Why we need to write parallel programs

An efficient parallel implementation of a serial program might not be obtained by finding efficient parallelizations of each of its steps. Rather, the best parallelization might be obtained by stepping back and devising an entirely new algorithm.

## 2.1 Some Background

### 2.1.1 The von Neumann architecture
The separation of memory and CPU is often called the **von Neumann bottleneck**, since the interconnect determines the rate at which instructions and data can be accessed.

## 2.2 Modifications to the von Neumann Model

### 2.2.1 The basics of caching

**Cache**\
In general a cache is a collection of memory locations that can be accessed in less time than some other memory locations.

**CPU cache**\
A collection of memory locations that the CPU can access more quickly than it can access main memory.

**Cache eviction**\
When more than one line in memory can be mapped to several different locations
in a cache, we need to be able to decide which line in the cache should be replaced. The most commonly used scheme is called **least recently used**. The cache has a record of the relative order in which the blocks have been used, and the least recently used line is evicted and replaced by a new line from memory.

## 2.3 Parallel hardware

### Flynn's taxonomy

|                       | Single Instruction | Multi Instructioons | Single Program |
|-----------------------|--------------------|---------------------|----------------|
| Single Data Stream    | SISD               | MISD                |                |
| Multiple Data Streams | SIMD (SIMT)        | MIMD                | SPMD           |

**SISD**\
Single Instruction Single Data: your average “dumb” single-core processors. They process instructions serially, and each instruction is executed on one piece of data.

Good examples are microcontrollers, which tend to be single core machines.

**SIMD**\
SIMD instructions are classically called “**vector processors**”. The idea is that arithmetic operations tend to be executed many times on different pieces of data. Instead of creating separate CPU cores, you share control logic such as the instruction decoder, and execute the instruction on multiple pieces of data simultaneously.

Nowadays, you will find this style of instruction integrated into modern processors. On x86, you will find these instructions in the SSE, MMX, and AVX extensions.

SIMD processors execute their instructions in lockstep. This means that each instruction performs multiple additions/multiplications/whatnot, they MUST all be executed at once (or wait until all can be executed).

**SIMT**\
A variation of SIMD machines is the Single Instruction Multiple Thread category. This was not present in the taxonomy Flynn proposed, but has implicitly been added.

SIMT is intended to limit instruction fetching overhead, i.e. the latency that comes with memory access, and is used in modern GPUs

Note that SIMD and SIMT are nearly equivalent. The only difference is that in SIMT, each processed value belongs to a separate thread.

**MISD**\
MISD is a very rare processor type. One of the main places you will find it “in the wild” is in systems that HAVE to be reliable, such as satellites. It’s pretty hard to fly out in space to service one, so once it’s up there, it HAS to keep working. They often have multiple processors executing the same instructions, where the results are compared to make sure no fault has occurred, which can happen due to factors such as interstellar radiation.

**MIMD**\
CPUs have multiple cores nowadays. Each core can execute instructions independently, and cores can work on different pieces of data.

**SPMD**\
Single Program Multiple Data is essentially MPI (Message Passing Interface); you run the same program on multiple physical machines, usually in the context of a supercomputer. These individual instances in turn execute the Same Program on Different pieces of Data.

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

For instance, when computing a problem on a distributed-memory system, both the number of processors and the size of accumulated caches from different processors increases. With the larger accumulated cache size, more or even all of the working set can fit into caches and the memory access time can decrease dramatically, which causes extra speedup in addition to that from the actual computation.

For instance, a program that must use secondary storage for its data when it’s run on a single processor system might be able to fit all of its data into main memory when run on a large distributed-memory system.

Theoretical spuperlinear speedup is a contradiction because speedup, by definition is computed with respect to the best sequential algorithm.

**Efficiency**\
If S is the Speedup and P is the number of processes, the efficiency is given by **E = S/P**

**Parallel overhead**\
In general, dividing the work of a serial program among the processes/threads introduces a “parallel overhead” such as mutual exclusion or communication. If **T<sub>overhead</sub>** denotes this parallel overhead, it’s often the case that **T<sub>parallel</sub> = T<sub>serial</sub>/p + T<sub>overhead</sub>**.

### 2.6.2 Amdahl's law

**Amdahls law**\
If a fraction **r** of our serial program remains unparallelized, then Amdahl’s law says we can’t get a speedup better than **1/r**.\
**S<sub>latency</sub>(s) = 1&frasl;<sub>(1-p+<sup>p</sup>&frasl;<sub>s</sub>)</sub>**

**Gustafson's law**\
Amdahl's law applies only to cases where the problem size is fixed. Gustafson argues that as more computing resources become available, they tend to get used on larger datasets, and the time spent in the parallelizable part often grows much faster than the time spent in parts which have to be executed in serial. This means that as the number of processes increase, the speedup achieved by parallelization might actually exceed the maximum speedup calculated by Amdahls law, since the increased data size means that the fraction of the program which has to be executed in serial has decreased for this larger data set. Therefore, Gustafson's law gives a less pessimistic and more realistic assessment of the parallel performance.\
**S<sub>latency</sub>(s) = (1 - p) + N p**