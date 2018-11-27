# POSIX threads

**What is pthreads**\
POSIX (Portable Operating System Interface) Threads, usually referred to as pthreads, is an execution model that exists independently from a language, as well as a parallel execution model. pthreads defines a set of C programming language types, functions and constants. It is implemented with a pthread.h header and a thread library.

**Nondeterminism**\
Any time the processors execute asynchronously, there is the potential for **nondeterminism**, that is, for a given input the behavior of the program can change from one run to the next. This can be a serious problem, especially in shared-memory programs.

**Race conditions**\
If the nondeterminism results from two threads’ attempts to access the same resource, and it can result in an error, the program is said to have a **race condition**.

**Critical sections**\
A critical section happens when you have shared data between multiple threads or processes. We need to protect access to commonly accessed variables in critical sections.

**Pthread's read-write locks**\
A read-write lock can be used to protect a shared resource that can be either read or written to (modified). All reader threads can access it simultaneously. A writer thread needs exclusive access.

The advantage of read-write locks is that they do a very god job of allowing concurrent access to data.

**Initialize a pthread mutex**
```C
// Initialize a pthread mutex
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```
```C
// Lock the initialized mutex
pthread_mutex_lock(&mutex);
```
```C
// Unlock the locked mutex
pthread_mutex_unlock(&mutex);
```
Whatever piece of code is between the mutex lock and the mutex unlock can only be executed by one thread at once.

**Mutexes can cause deadlocks**\
One thing to note about mutexes is that a mutex cannot be locked by one thread if it's already locked by another thread. If one thread tries to lock a mutex which is already locked, it will wait for the mutex to be unlocked. If multiple threads use multiple locks, a deadlock might occur.
```C
// Can deadlock
if (thread_id % 2 == 0) {
    pthread_mutex_lock(&mutex_a);
    pthread_mutex_lock(&mutex_b);
    a++;
    b++;
    pthread_mutex_unlock(&mutex_b);
    pthread_mutex_unlock(&mutex_a);
} else {
    pthread_mutex_lock(&mutex_b);
    pthread_mutex_lock(&mutex_a);
    a++;
    b++;
    pthread_mutex_unlock(&mutex_a);
    pthread_mutex_unlock(&mutex_b);
}
```
Consider the code above. Let's say we have two threads, thread_1 and thread_2. A deadlock can occur if thread_1 locks mutex_a, and thread_2 locks mutex_b before thread_1 gets to lock it. Then thread_1 will have to wait for mutex_b to be unlocked, and thread_2 will have to wait for mutex_a to be unlocked, neither of which will ever happen, since the two threads are mutually awaiting each other.

**Preventing a deadlock**
```C
// Cannot deadlock
if (thread_id % 2 == 0) {
    pthread_mutex_lock(&mutex_a);
    pthread_mutex_lock(&mutex_b);
    a++;
    b++;
    pthread_mutex_unlock(&mutex_a);
    pthread_mutex_unlock(&mutex_b);
} else {
    pthread_mutex_lock(&mutex_a);
    pthread_mutex_lock(&mutex_b);
    a++;
    b++;
    pthread_mutex_unlock(&mutex_a);
    pthread_mutex_unlock(&mutex_b);
}
```
In the piece of code above, we force all the threads to obtain locks in the same order. This prevents the first line of one thread's code block to block the last lines of another thread's code block. This effectively prevents a deadlock. Note that when preventing deadlocks with MPI, we want the communicating threads to reach a send/receive statement in mutually opposite order. When dealing with mutexes, however, we want to make sure all threads obtain locks in the same order. Note that in the example above, the if/else isn't really needed.

```C
// Can deadlock
if (thread_id % 3 == 0) {
    pthread_mutex_lock(&mutex_a);
    pthread_mutex_lock(&mutex_c);
    a++;
    c++;
    pthread_mutex_unlock(&mutex_c);
    pthread_mutex_unlock(&mutex_a);
} else if ( thread_id % 3 == 1) {
    pthread_mutex_lock(&mutex_b);
    pthread_mutex_lock(&mutex_a);
    a++;
    b++;
    pthread_mutex_unlock(&mutex_a);
    pthread_mutex_unlock(&mutex_b);
} else {
    pthread_mutex_lock(&mutex_c);
    pthread_mutex_lock(&mutex_b);
    c++;
    b++;
    pthread_mutex_unlock(&mutex_b);
    pthread_mutex_unlock(&mutex_c);
}
```
The code above shows another deadlock example. Let's say we have thread_1, thread_2 and thread_3. With some bad luck, thread_1 might lock mutex_a, thread_3 might lock mutex_c and thread_2 might lock mutex_b. Then thread_1 cannot lock mutex_c, thread_3 cannot lock mutex_b and thread_2 cannot lock mutex_a. We have a deadlock.