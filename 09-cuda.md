# CUDA

**Coalescing memory on GPUs**\
Memory coalescing is a technique which allows for optimal usage of the global memory bandwidth. When parallel threads running the same instruction access to consecutive locations in the global memory, the most favorable access pattern is achieved.

Perfectly coalesced memory transactions of a single warp should all land on the same cache line. Note that the location within the cache line doesn’t matter there. So if your threads are reading addresses in reverse, that’s totally fine too.