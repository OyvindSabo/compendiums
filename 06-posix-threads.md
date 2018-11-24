# POSIX threads

**Pthread's read-write locks**\
A read-write lock can be used to protect a shared resource that can be either read or written to (modified). All reader threads can access it simultaneously. A writer thread needs exclusive access.

The advantage of read-write locks is that they do a very god job of allowing concurrent access to data.