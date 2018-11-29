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

## 4.8 Barriers and a condition variable

A problem in shared-memory programming is synchronizing the threads by making sure that they all are at the same point in a program. Such a point of synchronization is called a **barrier** because no thread can proceed beyond the barrier until all the threads have reached it.

A very important use of barriers is in debugging.

Many implementations of Pthreads don’t provide barriers, so if our code is to be portable, we need to develop our own implementation. There are a number of options. We’ll look at three.
- Busy-waiting and a mutex
- Semaphores
- Condition variables

### 4.8.1 Busy-waiting and a mutex

we use a shared counter protected by the mutex. When the counter indicates that every thread has entered the critical section, threads can leave a busy-wait loop.
```C
// Shared and initialized by the main thread 
int counter; // Initialize to 0
int thread count;
pthread mutex t barrier mutex;

. . .

void∗ Thread work(. . .) {
    
    . . .
    
    /∗ Barrier ∗/
    pthread mutex lock(&barrier mutex);
    counter++;
    pthread mutex unlock(&barrier mutex);
    while (counter < thread count);
    
    . . .

}
```
Problems with this approach:
- We’ll waste CPU cycles when threads are in the busy-wait loop.
- Introduces problems when we reuse the counter for multiple barriers, so we need one counter variable for each barrier.

### 4.8.2 Semaphores
```C
// Shared variables
int counter; // Initialize to 0
sem_t count_sem; // Initialize to 1
sem_t barrier_sem; // Initialize to 0

. . .

void∗ Thread_work(...) {

    . . .

    // Barrier
    sem_wait(&count_sem);
    // The last thread will resent counter and unlock barrier
    if (counter == thread_count−1) {
        counter = 0;
        sem_post(&count sem);
        for (j = 0; j < thread_count−1; j++)
            sem post(&barrier_sem);
    // The other threads will unlock count_sem and increase the lock of barrier_sem
    } else {
        counter++;
        sem_post(&count_sem);
        sem_wait(&barrier_sem);
    }

    . . .

}
```
Note that it doesn’t matter if the thread executing the loop of calls to `sem_post(&barrier_sem)` races ahead and executes multiple calls to sem post before a thread can be unblocked from `sem_wait(&barrier_sem)`.'

What's great about this approach?
- This implementation of a barrier is superior to the busy-wait barrier, since the threads don’t need to consume CPU cycles when they’re blocked in in `sem_wait`.
- We can reuse `counter` and `count_sem`. Reusing barrier_sem might result in a race condition.

### 4.8.3 Condition variables

A better approach to creating a barrier in Pthreads is provided by condition variables. A condition variable is a data object that allows a thread to suspend execution until a certain event or condition occurs. When the event or condition occurs
another thread can signal the thread to “wake up.” A condition variable is always associated with a mutex.

```C
// Shared
int counter = 0;
pthread_mutex_t mutex;
pthread_cond_t cond_var;

. . .

void∗ Thread work(. . .) {

    . . .

    // Barrier
    pthread_mutex_lock(&mutex);
    counter++;
    if (counter == thread_count) {
        counter = 0;
        pthread_cond_broadcast(&cond_var);
    } else {
        while (pthread_cond_wait(&cond_var, &mutex) != 0);
    }
    pthread_mutex_unlock(&mutex);

. . .

}
```
It is possible that events other than the call to `pthread_cond_broadcast`
can cause a suspended thread to unblock. Hence, the call to pthread cond wait is usually placed in a while loop, so that the unblocked thread will call `pthread_cond_wait` again.

### 4.8.4 Pthreads barriers
The Open Group, the standards group that is continuing to develop the POSIX standard, does define a barrier interface for Pthreads, but it's not universally available.