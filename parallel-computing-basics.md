### The challenges of computing are changing**

**The Power wall**\
**Before:** Power is free, but transistors are expensive.\
**Now:** Power is expensive, but transistors are “free”.

**The Memory wall**\
**Before:** Multiply is slow, but load and store is fast.\
**Now:** Load and store is slow, but multiply is fast.

**The ILP wall**\
**Before:** We can reveal more instruction-level parallelism (ILP) via compilers and architecture innovation.\
**Now:** There are diminishing returns on finding more ILP.

The main challenges we are faced with today in computing, consist of the **Power Wall**, the **Memory Wall** and the **ILP Wall**. Together, these "walls" have been coined the **Brick wall** by Prof. David Patterson at UC Berkeley.

`Power Wall + Memory Wall + ILP Wall = Brick Wall`

### Most common performance issues in parallel programs according to Microsoft

- **Amount of Parallelizable CPU-Bound Work:** The number one requirement for parallelization is that the program must have enough work that can be performed in parallel. If only half of the work can be parallelized, Amdahl’s Law dictates that we are not going to be able to speed up the program by more than a factor of two. 
- **Task Granularity:** Even if a program does a lot of parallelizable work, we must be careful to ensure that we will split up the work into appropriately-sized chunks which will execute in parallel. If we create too many chunks, the overheads of managing and scheduling the chunks will be large. If we create too few chunks, some cores on the machine will have nothing to do.
- **Load Balancing:** Even if there is enough parallelizable CPU work to make parallelism worthwhile, we need to ensure that the work will be evenly distributed among cores on the machine. This is complicated by the fact that different “chunks” of work may differ widely in the time required to execute them. Also, we often don’t know how much work each chunk will require until we execute it till completion.
- **Memory Allocations and Garbage Collection:** Some programs spend a lot of time in memory allocations and garbage collections. Unfortunately, allocating memory is an operation that may require synchronization. After all, we need to ensure that memory regions allocated by different threads will not overlap. Perhaps even more seriously, allocating a lot of memory typically means that we will also need to do a lot of garbage collection work to reclaim memory that has been freed. If the garbage collection dominates the running time of your program, the program will only scale as well as the garbage collection algorithm.
- **False Cache-Line Sharing:** If one core invalidates a particular memory location, the version of that memory location cached by another core gets invalidated. Then, the core with an invalid cached copy must go all the way to the main memory on the next read of that memory location. So, if two cores keep writing and reading a particular memory location, they may end up continuously invalidating each other’s caches, sometimes dramatically reducing the performance of the program.
- **Locality Issues:** Sometimes modifying a program to run in parallel negatively affects locality. For example, let’s say that we want to perform an operation on each element on an array. On a dual-core machine, we could create two tasks: one to perform the operation on the elements with even indices and one to handle odd indices. As a negative consequence, the locality of reference degrades for each thread. 

### The difference between processes and threads

**Processes:**\
A process is an instance of a computer program.\
of a computer program that is being executed.\
Processes run in separate memory spaces.\
Each process is started with a single thread, often called the primary thread, but can create additional threads from any of its threads.

**Threads:**\
Threading provides a mechanism for programmers to divide their programs into more or less independent tasks with the property that when one thread is blocked another thread can be run. Furthermore, in most systems it’s possible to switch between threads much faster than it’s possible to switch between processes. This is because threads are “lighter weight” than processes.\
While two threads belonging to one process can share most of the process’ resources. The two mos important exceptions are that they’ll need a record of their own program counters
and they’ll need their own call stacks so that they can execute independently of each
other.\
Threads run in a shared memory space.\
A thread is an entity within a process.\
Each thread shares the memory space of their owning process.

### Instruction level parallellism (ILP)

Without instruction-level parallelism, a processor can only issue less than one instruction per clock cycle (IPC < 1). These processors are known as subscalar processors. These instructions can be re-ordered and combined into groups which are then executed in parallel without changing the result of the program. This is known as instruction-level parallelism.

**Pipelining**\
All modern processors have multi-stage instruction pipelines. Each stage in the pipeline corresponds to a different action the processor performs on that instruction in that stage; a processor with an N-stage pipeline can have up to N different instructions at different stages of completion and thus can issue one instruction per clock cycle (IPC = 1). These processors are known as scalar processors.

| Clock Cycle | 1     | 2     | 3     | 4     | 5     | 6     | 7     | 8     | 9     |
|-------------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| 1           | Fetch | Deco  | Exec  | Mem   | Write |       |       |       |       |
| 2           |       | Fetch | Deco  | Exec  | Mem   | Write |       |       |       |
| 3           |       |       | Fetch | Deco  | Exec  | Mem   | Write |       |       |
| 2           |       |       |       | Fetch | Deco  | Exec  | Mem   | Write |       |
| 3           |       |       |       |       | Fetch | Deco  | Exec  | Mem   | Write |
*Pipelining takes advantage of the computer's ability to run different instructions at the same time.*

**Multiple issue**\
Multiple issue processors replicate functional
units and try to simultaneously execute different instructions in a program.
```C
for (i = 0; i < 1000; i++)
{
  z[i] = x[i] + y[i];
}
```
Consider the for loop above. If we have N complete floating point adders, we can approximately make the execution of the for loop N times faster, since each calculation is independent from the others and can be run concurrently.

If the functional units are scheduled at compile time, the multiple issue system is said to use static multiple issue. If they’re scheduled at run-time, the system is said to use dynamic multiple issue. A processor that supports dynamic multiple issue is sometimes said to be superscalar (not the same as superlinear).

**Speculation**\
A common term for the techniques used in multiple issue to determine whether individual instructions are independent of each other.