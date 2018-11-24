# OpenMP

# 5 Shared Memory Programming with OpenMP

Like Pthreads, OpenMP is an API for shared-memory parallel programming. The “MP” in OpenMP stands for “multiprocessing,” a term that is synonymous with shared-memory parallel computing. Thus, OpenMP is designed for systems in which each thread or process can potentially have access to all available memory, and, when we’re programming with OpenMP, we view our system as a collection of cores or CPUs, all of which have access to main memory

OpenMP was explicitly designed to allow programmers to incrementally parallelize existing serial programs. This is virtually impossible with MPI and fairly difficult with Pthreads.

**OpenMP is not a library**\
Pthreads (like MPI) is a library of functions that can be linked to a C program, so any Pthreads program can be used with any C compiler, provided the system has a Pthreads library. OpenMP, on the other hand, extends the programming language's syntax and requires compiler support for some operations, and hence it’s entirely possible that you may run across a C compiler that can’t compile OpenMP programs into parallel programs. That being said, OpenMP uses **pragmas**. Pragmas are compiler directives. They don’t do anything, unless the compiler supports the particular feature they are using. If our compiler does not support OpenMP, the program will simply be executed in serial. If running the program in parallel is crucial, and the compiler does not support OpenMP, one should rather use MPI or Pthreads.

**Example on parallelizing a program with OpenMP**
```C
// Initial program
int main() {
    int sum = 0;
    for (int i = 0; i < 1000; i++) {
        sum += array[i];
    }
    return 0;
}
```
`g++ ompsample.cpp -o omp`
```C
#include <omp.h>

//Parallelized using OpenMP
int main() {
    int sum = 0;
    #pragma omp parallel for
    for (int i = 0; i < 10000000000; i++) {
        sum += array[i];
    }
    return 0;
}
```
`g++ ompsample.cpp -o omp -fopenmp`