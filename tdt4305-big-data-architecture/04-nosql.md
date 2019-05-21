# NoSQL

## Types of NoSQL databases

**Document databases:** [MongoDB, CouchDB]\
**Column databases:** [Apacha Cassandra]\
**Key-value stores:** [Redis, Couchbase Server]\
**Cache systems:** [Redis, Memcache]\
**Graph databases:** [Neo4J]

**Document databases**\
Stores data in the form of documents using well known formats like JSON, BSON (Binary JSON) or XML

**Column databases**\
Stores data in a table, but rather than grouping entries by row, the table is partitioned into columns, each of which is stored in its own file. Often, queries are concerned with values from a specific column, so in those cases, a column based database can prevent a lot of unnecessary data from having to be accessed.

**Key-value stores**\
A more simple data model based on fast access by key to an associated value. The value can be a record, an object or a more complex structure.

**Grap databases**\
Data is represented by nodes and edges which reference each other.

## Examples
a) We have a student database with the following tables in a ralation schema.

Student(__SNo__, Name, Email)\
Exam(__ENo__, CourseName, EDay, EMonth, EYear, Duration)\
ExamResult(__ExamNo__, __StudentNo__, Grade)

It is interesting to know what exams a particular student has taken I t is also interesting to know what students have taken a particular exam.

**How would you store this schema in HBase when using the DDI (denormaliza-
tion, duplication, intelligent keys) design principle to eanble the queries suggested above?**

Part of the DDI design principle is denormalizing schemas by, for example, duplicating data in more than one table so that, at read time, no further aggregation is required.

Since we want to get all the students who have taken a particular exam (most likely by course name), we add CourseName to the ExamResult table. This means that to get all the students for a particular exam, we only need the ExamResults table.

Since we also want to get all the grades of a particular student, we create a table called StudentResults with the fields StudentNo and CourseName as primary keys, and the keys StudentName (might not be strictly necessary) and Grade

As such, the relation schema can be denormalized like this:

Student(__SNo__, Name, Email)\
StudentResult(__ExamNo__, StudentNo, Grade)
Exam(__ENo__, CourseName, EDay, EMonth, EYear, Duration)\
ExamResult(__ExamNo__, __StudentNo__, StudentName, Grade)



**b) Describe how «sharding» happens in both MongoDB and Apache HBase (også kalt «auto-sharding»).**

Sharding is horizontally partitioning a database into multiple smaller, faster and more easily managable databases which don't share anything and which can be distributed across multiple servers.

In a database, the response time is assumed to grow exponentially if the size of the database and the transactions per unit increase linearly.

Distributing a database into shards means that the database  both reduces response time, and is cheaper, since it can be run on multiple commodity server, rather than requiring one huge, high super power server from the future.

Sharding can be done manually by for example deciding that all data connected to users from a specific country will be stored in a database located in that country, and data about users fromanother country will be stored in a separate database in another country.

**HBase**
HBase uses a concept called **auto sharding**, which simply means that tables are dynamically distributed by the system to different region servers when they become too large. A region is a contiguous range of rows stored together. In HBase, the regions will be dynamically split depending to regulate the size of each region.

Each region is served by one and only one region server. Initially all data is stored in one region. When it becomes to large, it is split into two more or less equal parts.

## The CAP Theorem

The **CAP theorem**, also named Brewer's theorem after computer scientist Eric Brewer, states that it is impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees:

**Consistency:**\
All the servers in the system will have the same data so anyone using the system will get the same copy regardless of which server answers their request

**Availability:**\
The system will always respond to a request (even if it's not the latest data or consistent across the system or just a message saying the system isn't working)

**Partition tolerance:**\
The system continues to operate as a whole even if individual servers fail or can't be reached.
