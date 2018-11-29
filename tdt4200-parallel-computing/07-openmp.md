# OpenMP

# 5 Shared Memory Programming with OpenMP

Like **pthreads**, **OpenMP** is an API for shared-memory parallel programming. The “MP” in **OpenMP** stands for “**multiprocessing**,” a term that is synonymous with shared-memory parallel computing. Thus, OpenMP is designed for systems in which each thread or process can potentially have access to all available memory, and, when we’re programming with OpenMP, we view our system as a collection of cores or CPUs, all of which have access to main memory

OpenMP was explicitly designed to allow programmers to incrementally parallelize existing serial programs. This is virtually impossible with MPI and fairly difficult with Pthreads.

OpenMP was initially published in 1997 for Fortran.

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

**Parallelize a single line**
```C
#pragma omp parallel
doSomething();
```

**Parallelize multiple lines**
```C
#pragma omp parallel
{
    doSomething();
}
```

**Find thread number and team size**
```C
#include <omp.h>
#include <iostream>

int main() {
    #pragma omp parallel
    {
        std::cout
            << "Thread " << omp_get_thread_num()
            << " out of " << omp_get_num_threads() << std::endl;
    }
    return 0;
}
```

**Force a specific number of spawned threads (default is available cores)**
```C
#include <omp.h>
#include <iostream>

int main() {
    #pragma omp parallel num_threads(5)
    {
        std::cout
            << "Thread " << omp_get_thread_num()
            << " out of " << omp_get_num_threads() << std::endl;
    }
    return 0;
}
```

**Force some parts of multi-threaded code to run in serial using `critical`**
```C
#include <omp.h>
#include <iostream>
int main() {
    int count = 0;
    #pragma omp parallel
    {
        for(int i = 0; i < 100000; i++) {
            #pragma omp critical
            {
                count++;
            }
        }
    }
    std::cout << count << std::endl;
    return 0;
}
```
**Force some parts of multi-threaded code to run in serial using `atomic`**
```C
#include <omp.h>
#include <iostream>
int main() {
    int count = 0;
    #pragma omp parallel
    {
        for(int i = 0; i < 100000; i++) {
            #pragma omp atomic
            {
                count++;
            }
        }
    }
    std::cout << count << std::endl;
    return 0;
}
```
An atomic operation has much lower overhead. Where available, it takes advantage on the hardware providing (say) an atomic increment operation; in that case there's no lock/unlock needed on entering/exiting the line of code, it just does the atomic increment which the hardware tells you can't be interfered with.

The upsides are that the overhead is much lower, and one thread being in an atomic operation doesn't block any (different) atomic operations about to happen. The downside is the restricted set of operations that atomic supports.

**Critical section:**
- Ensures serialisation of blocks of code.
- Can be extended to serialise groups of blocks with proper use of "name" tag.
- Slower!

**Atomic operation:**
- Is much faster!
- Only ensures the serialisation of a particular operation.

**Debugging an OpenMP program**\
```C
#pragma omp parallel for
for(int i = 0; i < 10; i++){
    for(int j = 0; j < 10; j++){
        array[i] += buffer[i*10 + j];
        array[j] -= buffer[i*10 + j];
    }
}
```
Given the program above, let's find out what's wrong with it.
Since we only use `#pragma omp parallel for` outside of the outer for-loop, only the outer for loop will be parallelized. That means that we will spawn ten processes, each of which executes the inner for loop with different values of `i`. That means that ten separate processes will try to set the value of `array[j]` at the same time, which will cause a race condition and make the program non-deterministic.

**Parallelize nested for loops**
```C
// Parallelize only the outer for loop
#pragma omp parallel for
for (int i=0;i<N;i++) { 
    for (int j=0;j<M;j++) {
        //do task(i,j)//
    }
}
```
```C
// Parallelize both the outer and the inner for loop
#pragma omp parallel for collapse(2)
for (int i=0;i<N;i++) { 
    for (int j=0;j<M;j++) {
        //do task(i,j)//
    }
}
```

**Prevent erroneous calculations caused by numerical instability with OpenMP**
```C
double count = .0;
#pragma omp parallel for
for (int ii = 0; ii < 400; ii++) {
    count += fourier_series(ii);
}
return count;
```
The code above will yield a different result each time because we use floats, because floating point operations are not commutative, that is, the order in which they are executed are not ambivalent.

```C
// I think this might work lol
double count = .0;
#pragma omp parallel for reduction(+:sum)
for (int ii = 0; ii < 400; ii++) {
    count += fourier_series(ii);
}
return count;
```