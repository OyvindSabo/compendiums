# CUDA: Warp cooperation

### Synchronization
CUDA warps may be synchronized. Almost like on a CPU, warps within a block can be scheduled in any order. If your warps need to do an operation in a particular order, you can use __syncthreads() as a barrier warps within the same block will wait on before continuing. Doing this excessively can hurt performance.

### Thread divergence

**Thread divergence**\
Threads from a block are bundled into fixed-size warps for execution on a CUDA core, and threads within a warp must follow the same execution trajectory. All threads must execute the same instruction at the same time. In other words, threads cannot diverge.

In an if-then-else statement. If some threads in a single warp evaluate to 'true' and others to 'false', then the 'true' and 'false' threads will branch to different instructions. Some threads will want proceed to the 'then' instruction, while others the 'else'.

While executing the then part, all threads that evaluated to false (e.g. the else threads) are effectively deactivated. When execution proceeds to the else condition, the situation is reversed. When some threads work their way through one path of a branch, the other threads in the warp are just idling. If your code is very heavy on branches, this can mean you effectively only use a very small part of the computing power available to you.

Note that thread divergene can also occur in for-loops if each thread is prompted to run a different number of iterations. In the case where one thread loops through the numbers 0 to 10 and another thread loops through the numbers 0 to 20, thread divergence will not occur before both threads have finished their first 10 iterations.

**Some examples on thread divergence**
```C
/*
Does not cause thread divergence, since all the treads within the same warp are
also within the same block, so either all of the threads within the same warp
will fulfill the condition, or none of them will fulfill the condition.
*/
if (blockIdx.x > 16) {
    foo();
} else {
    bar();
}
```
```C
/*
Causes thread divergence, since each thread has a different threadIdx.x, causing
the condition will evaluate to true for some threads and false for other
threads.
*/
if (threadIdx.x > 16) {
    foo();
} else {
    bar();
}
```
```C
/*
Causes thread divergence, since each thread has a different threadIdx.x, and
will run the loop a different number of times.
*/
for (int i = 0; i < threadIdx.x; i++) {
    foo();
}
```

**Thread divergence can cause deadlock**
```C
//my_Func_then and my_Func_else are some device functions
if (threadidx.x <16)
{
    myFunc_then();
    __syncthread();
}
else if (threadidx >=16)
{
    myFunc_else();
    __syncthread();
}
```
The first half of the warp will execute the then part, then wait for the second half of the warp to reach __syncthread(). However, the second half of the warp did not enter the then part; therefore, the first half of the warp will be waiting for them forever.

**How to prevent thread divergence**
- Replace branches as much as possible with non-branching variants. For instance, if the then and else clause of an if statement are mostly the same, you can “cancel out” parts of the computation using arithmetic operations instead.
- Use the built-in min, max, and abs wherever applicable, because they have hardware implementations due to their frequent occurrence.

### Latency hiding
A warp stalls when it is waiting for memory. The main idea of latency hiding is to use the time in which one warp is stalling to execute other ones. If we manage to fill our
entire timeline with warps, we can then reorder the timeline, such that each time a warp was executing is grouped together. Since the processor was active all the time, it has executed all warps start to finish WITHOUT HAVING TO WAIT FOR THEIR MEMORY REQUESTS. This is called “latency hiding”, and is the primary reason why the register file is a thing, and why it is
desirable to run many blocks on an SM at the same time.