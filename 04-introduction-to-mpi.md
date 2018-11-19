# Introduction to MPI

## What is MPI?

**MPI** = **M**essage **P**assing **I**nterface

You can think of MPI as a delivery service between processes. MPI enables processes to send a message, labeling it with size, destination, layout, contents, and to receive a waiting message, as well as placing contents in its own memory.

## Some simple examples

**Launch 4 simultaneous instances of a program without MPI:**
```
./myprogram & ./myprogram & ./myprogram & ./myprogram
```
**Launch 4 simultaneous instances of a program with MPI:**
```
mpirun -np 4 ./myprogram
```

**Simple example to access the current process rank, as well as the total number of processes (size):**
```C
#include <stdio.h>
#include <mpi.h>

int main ( int argc, char **argv )
{
    int rank, size;
    MPI_Init ( &argc, &argv );
    MPI_Comm_rank ( MPI_COMM_WORLD, &rank );
    MPI_Comm_size ( MPI_COMM_WORLD, &size );
    printf ( "Hello from process %d out of %d\n", rank, size );
    MPI_Finalize();
}
```
**Compile and run the program in C:**
```
% mpicc -o hello_spmd hello_spmd.c
% mpirun -np 4 ./hello_spmd
Hello from process 0 out of 4
Hello from process 2 out of 4
Hello from process 1 out of 4
Hello from process 3 out of 4
```

**Compile and run the program in C++:**
```
% mpicxx -o hello_spmd hello_spmd.c
% mpirun -np 4 ./hello_spmd
Hello from process 0 out of 4
Hello from process 2 out of 4
Hello from process 1 out of 4
Hello from process 3 out of 4
```
**To compile and run in C++ you could also write:**
```
% mpic++ -o hello_spmd hello_spmd.c
% mpirun -np 4 ./hello_spmd
Hello from process 0 out of 4
Hello from process 2 out of 4
Hello from process 1 out of 4
Hello from process 3 out of 4
```