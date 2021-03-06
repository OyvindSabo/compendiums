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

As the microprocessor processes data, it looks first in the cache memory. If it finds the instructions or data it's looking for there from a previous reading of data, it does not have to perform a more time-consuming reading of data from larger main memory or other data storage devices. Cache memory is responsible for speeding up computer operations and processing.

Once they have been opened and operated for a time, most programs use few of a computer's resources. That's because frequently re-referenced instructions tend to be cached. This is why system performance measurements for computers with slower processors but larger caches can be faster than those for computers with faster processors but less cache space.

**CPU cache**\
A collection of memory locations that the CPU can access more quickly than it can access main memory.

**The different levels of cache**\
Cache memory is fast and expensive. Traditionally, it is categorized as "levels" that describe its closeness and accessibility to the microprocessor.

- **L1 cache (~32 KB)**, or primary cache, is extremely fast but relatively small, and is usually embedded in the processor chip as CPU cache.
- **L2 cache (~256 KB)**, or secondary cache, is often more capacious than L1. L2 cache may be embedded on the CPU, or it can be on a separate chip or coprocessor and have a high-speed alternative system bus connecting the cache and CPU. That way it doesn't get slowed by traffic on the main system bus.
- **L3 cache (~10 MB)** is specialized memory developed to improve the performance of L1 and L2. L1 or L2 can be significantly faster than L3, though L3 is usually double the speed of RAM. With multicore processors, each core can have dedicated L1 and L2 cache, but they can share an L3 cache. If an L3 cache references an instruction, it is usually elevated to a higher level of cache.

**Sample times comparing how long certain operations take**
| Level                   | Time     |
|-------------------------|----------|
| CPU Cycle (4.3 GHz)     | 0.23 ns  |
| L1 Cache Access         | 0.93 ns  |
| L2 Cache Access         | 3.26 ns  |
| L3 Cache Access         | 18.4 ns  |
| RAM Access              | 68.4 ns  |
| Intel Optane Access     | 14.9 μs  |
| NVMe SSD Access         | 338 μs   |
| SATA SSD Access         | 2.0 ms   |
| Mechanical HDD Access   | 7.6 ms   |
| Ping to uio.no          | 14.4 ms  |
| Ping to ucla.edu        | 189.8 ms |
| Windows installs update | 40 m     |


**Locality**\
Once we have a cache, an obvious problem is deciding which data and instructions should be stored in the cache. The universally used principle is based on the idea that programs tend to use data and instructions that are physically close to recently used data and instructions. After executing an instruction, programs typically execute the next instruction; **branching** tends to be relatively rare. Similarly, after a program has accessed one memory location, it often accesses a memory location that is physically nearby.

The principle that an access of one location is followed by an access of a nearby location is often called locality. After accessing one memory location (instruction or data), a program will typically access a nearby location (spatial locality) in the near future (temporal locality).

Much of the effort in getting efficient code is in adapting it to keep the CPU supplied. If the CPU is not busy, the code is inefficient.

**Some examples on locality**
```C
/*
Utilizes spatial locality because consequtive memory locations are accessed for
every iteration of the for loop.
*/
for (int i = 0; i < 10000; i++) {
    a[i] = b[i] + c[i];
}
```
```C
/*
Utilizes spatial and temporal locality because consequtive memory locations are
accessed for every iteration of the inner for loop, and the same memory
locations are accessed for every iteration of the outer for loop.
*/
for(int i = 0; i < 100; i++) {
    for(int j = 0; j < 100; j++) {
      a[j] = b[j] + c[j];
    }
}
```
```C
/*
Utilizes temporal locality because the same memory locations are accessed for
every iteration of the outer for loop
*/ 
for(int i = 0; i < 100; i++) {
    for(j = 0; j < 10; j++) {
        a[j*1000] += b[j * 2000] + c[j * 3000];
    }
}
```

**How to write efficient code with regards to cache**
- Don't swap out of memory - reading from a hard disk takes “forever”.
- Don't swap out of L2 cache (bring data into L2 cache in blocks).
- Don't swap out of L1 cache.
- Don't swap out of registers.

**Cache memory mapping**
- **Direct mapped cache** has each block mapped to exactly one cache memory location. Conceptually, direct mapped cache is like rows in a table with three columns: the data block or cache line that contains the actual data fetched and stored, a tag with all or part of the address of the data that was fetched, and a flag bit that shows the presence in the row entry of a valid bit of data.
- **Fully associative cache** mapping is similar to direct mapping in structure but allows a block to be mapped to any cache location rather than to a prespecified cache memory location as is the case with direct mapping.
- **Set associative cache** mapping can be viewed as a compromise between direct mapping and fully associative mapping in which each block is mapped to a subset of cache locations. It is sometimes called N-way set associative mapping, which provides for a location in main memory to be cached to any of "N" locations in the L1 cache.

**Cache eviction**\
When more than one line in memory can be mapped to several different locations
in a cache, we need to be able to decide which line in the cache should be replaced. The most commonly used scheme is called **least recently used**. The cache has a record of the relative order in which the blocks have been used, and the least recently used line is evicted and replaced by a new line from memory.

### 2.2.6 Hardware multithreading

**Granularity**\
G = <sup>T<sub>computation</sub></sup> &frasl;<sub>T<sub>communication</sub></sub>

**Fine-grained parallelism**\
A program is broken down to a large number of small tasks. These tasks are assigned individually to many processors. The amount of work associated with each parallel task is low and the work is evenly distributed among the processors. Hence, fine-grained parallelism facilitates load balancing.

As each task processes less data, the number of processors required to perform the complete processing is high. This in turn, increases the communication and synchronization overhead.

Fine-grained parallelism is best exploited in architectures which support fast communication. Shared memory architecture which has a low communication overhead is most suitable for fine-grained parallelism.

It is difficult for programmers to detect parallelism in a program, therefore, it is usually the compilers' responsibility to detect fine-grained parallelism.

**Coarse-grained parallelism**\
In coarse-grained parallelism, a program is split into large tasks. Due to this, a large amount of computation takes place in processors. This might result in load imbalance, wherein certain tasks process the bulk of the data while others might be idle. Further, coarse-grained parallelism fails to exploit the parallelism in the program as most of the computation is performed sequentially on a processor. The advantage of this type of parallelism is low communication and synchronization overhead.

Message-passing architecture takes a long time to communicate data among processes which makes it suitable for coarse-grained parallelism

## 2.3 Parallel hardware

### 2.3.4 Cache coherence

**Snooping cache coherence**\
The idea behind snooping comes from bus-based systems: When the cores share a bus, any signal transmitted on the bus can be “seen” by all the cores connected to the bus. Thus, when core 0 updates the copy of x stored in its cache, if it also broadcasts this information across the bus, and if core 1 is “snooping” the bus, it will see that x has been updated and it can mark its copy of x as invalid. This is more or less how snooping cache coherence works. The principal difference between our description and the actual snooping protocol is that the broadcast only informs the other cores that the cache line containing x has been updated, not that x has been updated.

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

The principal distinction between different MIMD systems is whether they are shared-memory or distributed-memory systems. In shared-memory systems, each processor or core can directly access every memory location, while in distributed-memory systems, each processor has its own private
memory

**SPMD**\
Single Program Multiple Data is essentially MPI (Message Passing Interface); you run the same program on multiple physical machines, usually in the context of a supercomputer. These individual instances in turn execute the Same Program on Different pieces of Data.

## 2.4 Parallel software

### 2.4.3 Shared-memory

**Mutexes**\
The most commonly used mechanism for insuring mutual exclusion is a mutual exclusion lock or mutex or lock. A mutex is a special type of object that has support in the underlying hardware. The basic idea is that each critical section is protected by a lock. Before a thread can execute the code in the critical section, it must “obtain” the mutex by calling a mutex function, and, when it’s done executing the code in the critical section, it should “relinquish” the mutex by calling an unlock function. While one thread “owns” the lock—that is, has returned from a call to the lock function, but hasn’t yet called the unlock function—any other thread attempting to execute the code in the critical section will wait in its call to the lock function.

**Busy-waiting**\
There are alternatives to mutexes. In busy-waiting, a thread enters a loop whose sole purpose is to test a condition. This loop is called a “busy-wait” because the thread can be very busy waiting for the condition. This has the virtue that it’s simple to understand and implement. However, it can be very wasteful of system resources, because even when a thread is doing no useful work, the core running the thread will be repeatedly checking to see if the critical section can be entered.

**Semaphores**\
Semaphores are similar to mutexes, although the details of their behavior are slightly different, and there are some types of thread synchronization that are easier to implement with semaphores than mutexes.

`int sem_post(sem_t *sem)` - Unlock a semaphore\
**sem_post()** increments (unlocks) the semaphore pointed to by sem. If the semaphore's value consequently becomes greater than zero, then another process or thread blocked in a sem_wait(3) call will be woken up and proceed to lock the semaphore.

`int sem_wait(sem_t *sem);` - Lock a semapore\
**sem_wait()** decrements (locks) the semaphore pointed to by sem. If the semaphore's value is greater than zero, then the decrement proceeds, and the function returns, immediately. If the semaphore currently has the value zero, then the call blocks until either it becomes possible to perform the decrement (i.e., the semaphore value rises above zero), or a signal handler interrupts the call.

### 2.4.4 Distributed-memory

**One-sided commuication**\
One-sided communication is also called remote memory access. A single process calls a function, which updates either local memory with a value from another process or remote memory with a value from the calling process. This can simplify communication, since it only requires the active participation of a single process.

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
If S is the Speedup and P is the number of processes, the efficiency is given by\
**E = <sup>S</sup>&frasl;<sub>P</sub>**
In the case of speedup achieved by parallelization, this is equivalent to\
**E = <sup>T<sub>Serial</sub></sup>&frasl;<sub>T**<sub>Parallel * P</sub></sub>

**Parallel overhead**\
In general, dividing the work of a serial program among the processes/threads introduces a “parallel overhead” such as mutual exclusion or communication. If **T<sub>overhead</sub>** denotes this parallel overhead, it’s often the case that\
**T<sub>parallel</sub> = <sup>T<sub>serial</sub></sup>&frasl;<sub>p</sub> + T<sub>overhead</sub>**

### 2.6.2 Amdahl's law

**Amdahl's law**\
If a fraction **r** of our serial program remains unparallelized, then Amdahl’s law says we can’t get a speedup better than **1/r**.\
**S<sub>latency</sub>(s) = 1&frasl;<sub>(1-p+<sup>p</sup>&frasl;<sub>s</sub>)</sub>**

**Gustafson's law**\
Amdahl's law applies only to cases where the problem size is fixed. Gustafson argues that as more computing resources become available, they tend to get used on larger datasets, and the time spent in the parallelizable part often grows much faster than the time spent in parts which have to be executed in serial. This means that as the number of processes increase, the speedup achieved by parallelization might actually exceed the maximum speedup calculated by Amdahls law, since the increased data size means that the fraction of the program which has to be executed in serial has decreased for this larger data set. Therefore, Gustafson's law gives a less pessimistic and more realistic assessment of the parallel performance.\
**S<sub>latency</sub>(s) = (1 - p) + N p**

In other words, increasing the problem size can save us from Amdahl's law.

### 2.6.3 Scalability

**Scalable programs**\
Formally, a parallel program is scalable if there is a rate at which the problem size can be increased so that as the number of processes/threads is increased, the efficiency remains constant

**Strongly scalable programs**\
A program is strongly scalable, if the program can scale, while the problem size remains fixed. If you can achieve good speedup without increasing problemsize, you have strong scaling.

**Weakly scalable programs**\
A program is weakly scalable if the problem size needs to be increased at the same rate as the number of processes/threads.

**Note**\
Amdahl’s Law assumed a strict scaling setup (constant problem size in decreasing time) and Gustafson’s Law assumes a weak scaling setup (increasing problem size in fixed time).

## 2.10 Summary

### 2.10.2 Parallel hardware

**Branching**\
Branching in SIMD systems is handled by idling those processors that might operate on a data item to which the instruction doesn’t apply. This behavior makes SIMD systems poorly suited for task-parallelism, in which each processor executes a different task, or even data-parallelism, with many conditional branches.

### 2.10.3 Parallel software

**Branching**\
Most programs for MIMD systems consist of a single program that obtains parallelism by branching. Such programs are often called single program, multiple data or SPMD programs. Branching may lead to performance issues on both CPUs and GPUs.

**Threads and processes**\
In shared-memory programs we’ll call the instances of running tasks threads. In distributed-memory programs we’ll call them processes