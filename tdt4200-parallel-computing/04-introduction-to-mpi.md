# Introduction to MPI

## What is MPI?

**MPI** = **M**essage **P**assing **I**nterface

You can think of MPI as a delivery service between processes. MPI enables processes to send a message, labeling it with size, destination, layout, contents, and to receive a waiting message, as well as placing contents in its own memory.

In MPI, multiple copies of a Single program perform similar (although it might differ based on the data) computations on different data.

MPI is considered SPMD (**S**ingle **P**rogram **M**ultiple **D**ata), which is technically a subcategory of MIMD (**M**ultiple **I**nstruction **M**ultiple **D**ata) in Flynn's taxonomy. SPMD programs consist of a single executable that can behave as if it were multiple different programs through the use of conditional branches. The reason we classify MPI as SPMD and not MIMD is that it more explistly emphasizes that all the multiple instructions are instances of the same pogram.

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
When you call MPI_Init, it sets up a communicator called MPI_COMM_WORLD that
contains all of your processes

**MPI_Finalize**
Terminates the MPI execution environment. You should call this when you don't need the MPI anymore. The MPI standard requires that MPI_Finalize be called only by the same thread that initialized MPI with either MPI_Init or MPI_Init_thread.

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
- **elements**: The number of elements to be sent
- **type**: The MPI_Type can be MPI_INT, MPI_DOUBLE, MPI_BYTE, ...
- **destination**: The rank of the receiving process.
- **source**: The rank of the sending process from which a message is expected.
- **tag**: Just a random number you make up. It has to be the same at sender and receiver. It’s used for matching up the right pairs when you have several send/receive pairs at the same time.
- **communicator**: Collection of processes, for instance MPI_COMM_WORLD

**Advanced sending an receiving in MPI**\
The MPI_Recv() requires the user to specify a specific sender ID and a specific tag:
```C
MPI_Recv(buff, count, datatype, source, tag, communicator, status);
```

If you do not know in advance what values source and tag the message will be, you can use the wildcard values to receive the message, and then use the status variable to find out who the sender was:

```C
MPI_Recv(buff, count, type, MPI_ANY_SOURCE, MPI_ANY_TAG, communicator, status);
```

After the MPI_Recv() returns with a message, you can use these fields to find the source id of the sender and the tag of the message:

```C
status.SOURCE  = source id of sender;
status.TAG     = tag of message;
```




**Branching**\
The concept of MPI is based on the idea that a single program that can use branching to have multiple different behaviors.

**Deadlock**\
An `MPI_Send` will not proceed until a corresponding `MPI_Recv` has happened in the
receiving process (unless the buffer where the send buffer is being stored is cleared). If multiple processes are sending and receiving, in that particular order, we run the risk of both processes never initiating receiving, because they're mutually awaiting another process' revceive.

**Deadlock examples**
```C
// Deadlock
if (rank == 0) {
    MPI_Send(..., 1, tag, MPI_COMM_WORLD);
    MPI_Recv(..., 1, tag, MPI_COMM_WORLD, &status);
} else if (rank == 1) {
    MPI_Send(..., 0, tag, MPI_COMM_WORLD);
    MPI_Recv(..., 0, tag, MPI_COMM_WORLD, &status);
}
```
```C
// Deadlock prevented by reversing order of send and receive for one processes
if (rank == 0) {
    MPI_Send(..., 1, tag, MPI_COMM_WORLD);
    MPI_Recv(..., 1, tag, MPI_COMM_WORLD, &status);
} else if (rank == 1) {
    MPI_Send(..., 0, tag, MPI_COMM_WORLD);
    MPI_Recv(..., 0, tag, MPI_COMM_WORLD, &status);
}
```
```C
// Deadlock prevented by using non-blocking send (MPI_Isend)
if (rank == 0) {
    MPI_Isend(..., 1, tag, MPI_COMM_WORLD, &req);
    MPI_Recv(..., 1, tag, MPI_COMM_WORLD, &status);
    MPI_Wait(&req, &status)
} else if (rank == 1) {
    MPI_Send(..., 0, tag, MPI_COMM_WORLD);
    MPI_Recv(..., 0, tag, MPI_COMM_WORLD, &status);
}
```
**MPI_Isend prototype:**
```C
MPI_Isend (
    const void *buf,
    int count,
    MPI_Datatype datatype,
    int dest,
    int tag,
    MPI_Comm comm,
    MPI_Request *request)
```
Note that `MPI_Isend()` has an MPI_request parameter which is not present in the noemal `MPI_Send`. `MPI_Isend` initiates an asynchronous send operation. All asynchronous operations are given a request handle that has to be acted on later, for instace using MPI_Wait.

**MPI_Wait prototype:**
```C
MPI_Wait(
    MPI_Request *request,
    MPI_Status *status
)
```

**A list of all MPI send methods**\
MPI has a number of different "send modes." These represent different choices of buffering (where is the data kept until it is received) and synchronization (when does a send complete). In the following, "send buffer" is the user-provided buffer to send.

- **MPI_Send** MPI_Send will not return until you can use the send buffer. It may or may not block (it is allowed to buffer, either on the sender or receiver side, or to wait for the matching receive).
- **MPI_Bsend** May buffer; returns immediately and you can use the send buffer. A late add-on to the MPI specification. Should be used only when absolutely necessary.
- **MPI_Ssend** will not return until matching receive posted
- **MPI_Rsend** May be used ONLY if matching receive already posted. User responsible for writing a correct program.
- **MPI_Isend** Nonblocking send. But not necessarily asynchronous. You can NOT reuse the send buffer until either a successful, wait/test or you KNOW that the message has been received (see MPI_Request_free). Note also that while the **I**  refers to **I**mmediate, there is no performance requirement on MPI_Isend. An immediate send must return to the user without requiring a matching receive at the destination. An implementation is free to send the data to the destination before returning, as long as the send call does not block waiting for a matching receive. Different strategies of when to send the data offer different performance advantages and disadvantages that will depend on the application.
- **MPI_Ibsend** buffered nonblocking
- **MPI_Issend** Synchronous nonblocking. Note that a Wait/Test will complete only when the matching receive is posted.
- **MPI_Irsend** As with MPI_Rsend, but nonblocking

## Broadcasting

A broadcast is one of the standard collective communication techniques. During a broadcast, one process sends the same data to all processes in a communicator. One of the main uses of broadcasting is to send out user input to a parallel program, or send out configuration parameters to all processes.

One of the things to remember about collective communication is that it implies a synchronization point among processes. This means that all processes must reach a certain point in their code before they can all begin executing again. Collective operations do not have a tag argument. There is no tag because collectives are blocking, so you can only have one collective active at a time.

**MPI_Bcast**\
Makes sure that every rank gets a copy of some data from a root rank.

**MPI_Bcast prototype**
```C
int MPI_Bcast(
    void *buffer,
    int count,
    MPI_Datatype datatype,
    int root, // Rank of broadcast root
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

### Creating a custom MPI type

**Prototype for MPI_Type_vector:**
```C
MPI_Type_vector(
  int count,
  int blocklength,
  int stride, 
  MPI_Datatype oldtype,
  MPI_Datatype *newtype
)
```
**count:** number of blocks (nonnegative integer)\
**blocklength:** number of elements in each block (nonnegative integer)\
**stride:** number of elements between start of each block (integer)\
**oldtype:** old datatype (handle)