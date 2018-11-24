# CUDA: Warp cooperation

### Synchronization
CUDA warps may be synchronized. Almost like on a CPU, warps within a block can be scheduled in any order. If your warps need to do an operation in a particular order, you can use __syncthreads() as a barrier warps within the same block will wait on before continuing. Doing this excessively can hurt performance.

### Thread divergence

**Thread divergence**\
Threads from a block are bundled into fixed-size warps for execution on a CUDA core, and threads within a warp must follow the same execution trajectory. All threads must execute the same instruction at the same time. In other words, threads cannot diverge.

In an if-then-else statement. If some threads in a single warp evaluate to 'true' and others to 'false', then the 'true' and 'false' threads will branch to different instructions. Some threads will want proceed to the 'then' instruction, while others the 'else'.

While executing the then part, all threads that evaluated to false (e.g. the else threads) are effectively deactivated. When execution proceeds to the else condition, the situation is reversed. When some threads work their way through one path of a branch, the other threads in the warp are just idling. If your code is very heavy on branches, this can mean you effectively only use a very small part of the computing power available to you.

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