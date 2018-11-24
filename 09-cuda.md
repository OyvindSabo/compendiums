# CUDA

**Coalescing memory on GPUs**\
Memory coalescing is a technique which allows for optimal usage of the global memory bandwidth. When parallel threads running the same instruction access to consecutive locations in the global memory, the most favorable access pattern is achieved.

Perfectly coalesced memory transactions of a single warp should all land on the same cache line. Note that the location within the cache line doesn’t matter there. So if your threads are reading addresses in reverse, that’s totally fine too.

**How to achieve coalesced memory**
- Use structs of arrays, rather than arrays of sructs.

Consider the following two pieces of code
```C
int sum = 0;
for(int i = 0; i < n; i++){
    sum += array[i*n + thread_id];
}
```
```C
int sum = 0;
for(int i = 0; i < n; i++){
sum += array[n*thread_id + i];
 }
```
While the two pieces of code might seem to be equally efficient, the lower one better utilizes coalesced memory, since it reads consecutive values of an array. The top one skips sme values in the array, which means that the larger `n` grows, the more memory requests are required.

**Memory striding**\
Memory Striding is the act of accessing memory in an interleaved (discontinuous) manner. This results in an increased number of expensive memory accesses.