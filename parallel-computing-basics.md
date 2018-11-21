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

**Pipelining**\