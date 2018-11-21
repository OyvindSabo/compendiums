# OpenCL

**OpenCL**\
Originally released in 2009 by Apple for doing computations on the GPU’s of their computers. It was released as an open standard.

Currently OpenCL is the most cross-platform parallel computing language out there.

It basically runs on everything, except Apple hardware (soon).

OpenCl is a language. OpenCL ships source code with the program, and the runtime on the machine executing the kernels compiles it for whatever platform you want to run it on.

The recommended OpenCL standard to use is 2.0 to 2.2, since it's bet

**CUDA terminology vs OpenCL terminology**
| CUDA                     | OpenCL             |
|--------------------------|--------------------|
|                          | Platform           |
| Host                     | Host               |
| Device                   | Device             |
| Streaming Multiprocessor | Compute unit       |
| CUDA core                | Processing element |

**CUDA** is less verbose than **OpenCL**, but **OpenCL** is offered on a wider selection of devices.

Since **OpenCL** is platform agnostic, there’s no warplevel instrinsics like CUDA has.