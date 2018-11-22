# Introduction to MPI

## What is MPI?

**MPI** = **M**essage **P**assing **I**nterface

You can think of MPI as a delivery service between processes. MPI enables processes to send a message, labeling it with size, destination, layout, contents, and to receive a waiting message, as well as placing contents in its own memory.

MPI is considered SPMD (**S**ingle **P**rogram **M**ultiple **D**ata) in Flynn's taxonomy. In MPI, multiple copies of a Single program perform similar (although it might differ based on the data) computations on different data.

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

## Communicators

**MPI_Comm**\
A communicator, a collection of processes that you want
to group together.

**MPI_Init**\
When you call MPI_Init, it sets up a process called MPI_COMM_WORLD that
contains all of your processes

**MPI_Finalize**
Terminates the MPI execution environment. You should call this when you don't need the MPI anymore.

**MPI_COMM_WORLD**\
Collection of all your processes

**MPI_Comm_rank**

## Sending and receiving

**MPI_Send prototype:**
```C
MPI_Send (
    void *buffer,
    int elements,
    MPI_Type type,
    int destination,
    int tag,
    MPI_Comm communicator
);
```
**MPI_Recv prototype:**
```C
MPI_Recv (
    void *buffer,
    int elements,
    MPI_Type type,
    int source,
    int tag,
    MPI_Comm communicator,
    MPI_Status status
);

```
**What do the arguments mean?**
- **buffer** points to the transmitted data on the sending side. On the receiving side it points to a block of free memory where it will fit. If the sending side wants to transmit a variable called `myVariable`, **buffer** should be passed in as `&myVariable`. If the receiving side wants to store the received value in a variable called `yourVariable`, **buffer** should be set to `&yourVariable`
- **elements**: THe number of elements to be sent
- **type**: the MPI_Type can be MPI_INT, MPI_DOUBLE, MPI_BYTE, ...
- **destination**: The rank of the receiving process.
- **source**: The rank of the sending process from which a message is expected.
- **tag**: Just a random number you make up. It has to be the same at sender and receiver. It’s used for matching up the right pairs when you have several send/receive pairs at the same time.
- **communicator**: Collection of processes, for instance MPI_COMM_WORLD

## Broadcasting

**MPI_Bcast**\
Makes sure that every rank gets a copy of some data from a root rank.

**MPI_Bcast prototype**
```C
int MPI_Bcast(
    void *buffer,
    int count,
    MPI_Datatype datatype,
    int root,
    MPI_Comm comm
);
```

## Gathering

**MPI_Gather**\
Collects data from all ranks and gives it to the root rank.

**MPI_Allgather**\
Just like MPI_Gather, except that every rank ends up with all the collected data.

## Reduction

**MPI_Reduce**\
MPI_Reduce takes an array of input elements on each process and returns an array of output elements to the root process. The output elements contain the reduced result.
**MPI_Reduce prototype:**
```C
MPI_Reduce(
    void* send_data,
    void* recv_data,
    int count,
    MPI_Datatype datatype,
    MPI_Op op,
    int root,
    MPI_Comm communicator
)
```
It might be tempting to call MPI Reduce using the same buffer for both input and output. For example, if we wanted to form the global sum of x on each process and store the result in x on process 0, we might try calling\
`MPI Reduce(&x, &x, 1, MPI DOUBLE, MPI SUM, 0, comm);`\
However, this call is illegal in MPI, so its result will be unpredictable. This is
because the MPI Forum wanted to make the Fortran and C versions of MPI as similar
as possible, and Fortran prohibits aliasing. Two arguments are aliased if they refer to the same block of memory.