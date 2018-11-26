# CUDA

- CUDA is not C++
- NVCC has to support a particular compiler to compile the “host” code
- You may need to specify any architecture(s) you’d like to compile your kernels for.

**Coalescing memory on GPUs**\
Memory coalescing is a technique which allows for optimal usage of the global memory bandwidth. When parallel threads running the same instruction access to consecutive locations in the global memory, the most favorable access pattern is achieved.

Perfectly coalesced memory transactions of a single warp should all land on the same cache line. Note that the location within the cache line doesn’t matter there. So if your threads are reading addresses in reverse, that’s totally fine too.

**How to achieve coalesced memory**
- Use structs of arrays, rather than arrays of structs.

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

## CUDA Terminology
The CUDA API distinguishes the CPU from the GPU:\
Host: CPU side\
Device: GPU side

**GPU:**
```C
__device__ void doSomething();
```
```C
__global__ void mainKernel();
```
**CPU:**
```C
__host__ void cpuFunction();
```
Host and device functions cannot call each other.

## Writing a basic CUDA program

**Create a CUDA kernel**\
To create a kernel (a method which can be called by CUDA), simply add `__global__` before the function declaration:
```C
#include <stdio.h>

// Just a normal method
void runMeOnCPU() {
    printf("I'm a single threaded program");
}
```
```C
#include <stdio.h>

// CUDA kernel
__global__ void runMeOnGPU() {
    printf("I'm thread %i!\n" , threadIdx.x);
}
```

**Call the kernel**\
`kernel <<<num_blocks, threads_per_block>>> (args…)`\
This will invoke the kernel with **num_blocks** blocks of **threads_per_block** threads per block. **threads_per_block** can be as big as **1024**. Note that **threads_per_block** should be a multiple of 32. **num_blocks** can be as large as **65,535**.
```C
int main() {
    
    // Calling a normal method
    runMeOnCPU();

    return 0;
}
```
```C
int main() {
    
    // Calling the kernel with one block and 32 threads per block
    runMeOnGPU<<<1, 32>>>();
    
    return 0;
}
```

**Synchronize**\
Kernel launches are asynchronous, so the CPU can do other stuff after kicking off a job. If we want to make sure that all the threads have finished their computations before the CPU proceeds, we have to use `cudaDeviceSynchronize()`
```C
int main() {
    runMeOnGPU<<<1, 32>>>();

    // Make sure main does not return before all threads are done
    cudaDeviceSynchronize()

    return 0;
}
```

**Our finished example program:**
```C
#include <stdio.h>

__global__ void runMeOnGPU() {
    printf("I'm thread %i!\n" , threadIdx.x);
}
int main() {
    
    // call runMeOnGPU with 1 block, with 32 threads per block
    runMeOnGPU<<<1, 32>>>();


    cudaDeviceSynchronize();
    return 0;
}
```

**Compile the program**\
Any program with CUDA programming should be named: **xxxx.cu**

The compiler is **nvcc**, which can handle both C++ and CUDA code. It is capable of
compiling all C++ commands for the HOST and most for the DEVICE.

Compiling the program using `nvcc xxxx.cu -o xxxxx` yields xxxxx as an executable file.

If we call our program **myCudaProgram.cu**, we can compile it using the following command:
`nvcc myCudaProgram.cu -o myCompiledCudaProgram`

## Memory management

The GPU has its own memory banks, so memory needs to be allocated and
copied to the GPU explicitly

### Allocate memory
**cudaMalloc prototype**\
`cudaError_t cudaMalloc(void** devPtr, size_t size);`

GPU cudaMalloc is similar to CPU malloc. It only allocates space, but does not populate it with any data.

### Populate the allocated memory with data

**cudaMemcpy prototype**\
`cudaError_t cudaMemcpy(void* dest, const void* src, size_t count, cudaMemcpyKind kind);`

**Move memory from HOST to DEVICE**
```C
cudaMemcpy(device_bigArray,
           bigArray,
           sizeof(float) * length,
           cudaMemcpyHostToDevice);
```

**Move memory from DEVICE to HOST**
```C
cudaMemcpy(bigArray,
           device_bigArray,
           sizeof(float) * length,
           cudaMemcpyDeviceToHost);
```

### Free memory
`cudaError_t cudaFree(void** devicePointer);`

## Detecting bugs in a CUDA program

Given what we've gone through so far, let's see if we can debug a CUDA program.
```C
__global__ void average(float* in, float* out, int N) {
    int index = threadIdx.x + blockDim.x*blockIdx.x;
    int lindex = threadIdx.x;
    __shared__ float shared_array[66];
    if (index < N) {
        shared_array[index] = in[index];
        if (lindex == 0 && index != 0) {
            shared_array[0] = in[index-1];
        }
        if (lindex == 63 && index != N-1) {
            shared_array[65] = in[index + 1];
        }
        __syncthreads();
    }
    if (index == 0 || index == N-1) {
        out[index] = 0;
    } else if (index < N) {
        out[index] = (shared_array[lindex-1] +
                      shared_array[lindex] +
                      shared_array[lindex + 1])/3.0f;
    }
}

float* func(int N) {
    float* in_host = (float*)malloc(sizeof(float) * N);
    float* out_host = (float*)malloc(sizeof(float)* N);
    float* in_dev;
    float* out_dev;
    cudaMalloc((void**)&in_dev, sizeof(float)*N);
    cudaMalloc((void**)&out_dev, sizeof(float)*N);
    fill_input(in_host);
    cudaMemcpy(in_dev,
               in_host,
               sizeof(float)*N,
               cudaMemcpyHostToDevice);

    int nBlocks = N/64;
    if ( N % 64 != 0) {
        N++;
    }
    average<<<nBlocks, 64>>>(in_dev, out_dev, N);
    cudaMemcpy(out_host,
               out_dev,
               sizeof(float)*N,
               cudaMemcpyDeviceToHost);

    // The code does not free memory

    // If func is the main, then it should probably have a cudaDeviceSynchronize()

    return out_host;
}
```