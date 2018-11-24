# CUDA: Warp cooperation

### Synchronization
CUDA warps may be synchronized. Almost like on a CPU, warps within a block can be scheduled in any order. If your warps need to do an operation in a particular order, you can use __syncthreads() as a barrier warps within the same block will wait on before continuing. Doing this excessively can hurt performance.

### Thread divergence
Threads from a block are bundled into fixed-size warps for execution on a CUDA core, and threads within a warp must follow the same execution trajectory. All threads must execute the same instruction at the same time. In other words, threads cannot diverge.

In an if-then-else statement. If some threads in a single warp evaluate to 'true' and others to 'false', then the 'true' and 'false' threads will branch to different instructions. Some threads will want proceed to the 'then' instruction, while others the 'else'.

While executing the then part, all threads that evaluated to false (e.g. the else threads) are effectively deactivated. When execution proceeds to the else condition, the situation is reversed. As you can see, the then and else parts are not executed in parallel, but in serial. This serialization can result in a significant performance loss.