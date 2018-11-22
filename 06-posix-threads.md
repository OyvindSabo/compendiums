# POSIX threads

**Pthread's read-write locks**\
A read-write lock is somewhat like a mutex except that it provides two lock functions. The first lock function locks the read-write lock for reading, while the second locks it for writing. Multiple threads can thereby simultaneously obtain the lock by calling the read-lock function, while only one thread can obtain the lock by calling the write-lock function. Thus, if any threads own the lock for reading, any threads that want to obtain the lock for writing will block in the call to the write-lock function. Furthermore, if any thread owns the lock for writing, any threads that want to obtain the lock for reading or writing will block in their
respective locking functions.