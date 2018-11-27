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