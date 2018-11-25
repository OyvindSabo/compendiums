# POSIX threads

**Race conditions**\
A race condition occurs when the result of a computation depends on the order of execution, and this order is not deterministic.

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
// A potential deadlock
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