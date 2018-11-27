# OpenCL

**OpenCL**\
Originally released in 2009 by Apple for doing computations on the GPU’s of their computers. It was released as an open standard.

Currently OpenCL is the most cross-platform parallel computing language out there.

It basically runs on everything, except Apple hardware (soon).

OpenCl is a language. OpenCL ships source code with the program, and the runtime on the machine executing the kernels compiles it for whatever platform you want to run it on.

The recommended OpenCL standard to use is 2.0 to 2.2, since it's bet

**CUDA terminology vs OpenCL terminology**\
| CUDA                     | OpenCL             |
|--------------------------|--------------------|
|                          | Platform           |
| Host                     | Host               |
| Device                   | Device             |
| Streaming Multiprocessor | Compute unit       |
| CUDA core                | Processing element |

| CUDA          | OpenCL       |
|---------------|--------------|
| Grid          | NDRange      |
| Block         | Workgroup    |
| Thread        | Work-item    |
| Shared Memory | Local Memory |

**An old exam problem**\
CUDA & OpenCL(Match the values)

|                      | OpenCL Work-Item | OpenCL Work-Group | Processing Element |
|----------------------|------------------|-------------------|--------------------|
| AMD Stream Processor |                  |                   |                    |
| CUDA Core            |                  |                   | x                  |
| CUDA Thread block    |                  | x                 |                    |
| CUDA Thread          | x                |                   |                    |
| CUDA Warp            |                  |                   |                    | 

**CUDA** is less verbose than **OpenCL**, but **OpenCL** is offered on a wider selection of devices.

Since **OpenCL** is platform agnostic, there’s no warplevel instrinsics like CUDA has.

**Minimizing divergence for an OpenCl program**\
```C
// Consider the following OpenCL kernel
__kernel__ work()
int x = get_global_id(0);
int y = get_global_id(1);
if (x % 8 > 4) {
    func1();
} else {
    func2();
}

// It is launched like this
size_t global_work_size[2];
global_work_size[0] = 1024; global_work_size[1] = 1024;
size_t local_work_size[2];
local_work_size[0] = m;
local_work_size[1] = n;
clEnqueueNDRangeKernel(queue, kernel, 2, NULL, global_work_size, local_work_size, 0, NULL, NULL);
```
Let's consider four different value pairs for **n** and **m**, and see what minimizes thread divergence.

- n = 4, m = 16
- n = 32, m = 4
- n = 8, m = 8
- n = 4, m = 32

We need to choose the value pair which makes sure that all the threads in `work` enter the same if/else branch. `x` is dependent on the value returned from `get_global_id(0)`, which in turn is dependent on `local_work_size[0]`. Our only option to only enter one branch is to enter the else branch. To accomplish this, x % <= 4 for every x. That means that m cannot be greater than 4, leaving us with **n = 32, m = 4** as the only alternative not causing thread divergence.