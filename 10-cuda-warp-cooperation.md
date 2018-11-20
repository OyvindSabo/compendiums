# CUDA: Warp cooperation

### Synchronization
CUDA warps may be synchronized. Almost like on a CPU, warps within a block can be scheduled in any order. If your warps need to do an operation in a particular order, you can use __syncthreads() as a barrier warps within the same block will wait on before continuing. Doing this excessively can hurt performance.