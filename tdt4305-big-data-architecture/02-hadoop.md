# Hadoop

The main point of hadoop is basically just to store and process data quickly.

## Assumptions and Goals of Hadoop File System (HDFS)
The following is the unaltered description from (https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html)

**Hardware Failure**\
Hardware failure is the norm rather than the exception. An HDFS instance may consist of hundreds or thousands of server machines, each storing part of the file system’s data. The fact that there are a huge number of components and that each component has a non-trivial probability of failure means that some component of HDFS is always non-functional. Therefore, detection of faults and quick, automatic recovery from them is a core architectural goal of HDFS.

**Streaming Data Access**\
Applications that run on HDFS need streaming access to their data sets. They are not general purpose applications that typically run on general purpose file systems. HDFS is designed more for batch processing rather than interactive use by users. The emphasis is on high throughput of data access rather than low latency of data access. POSIX imposes many hard requirements that are not needed for applications that are targeted for HDFS. POSIX semantics in a few key areas has been traded to increase data throughput rates.

**Large Data Sets**\
Applications that run on HDFS have large data sets. A typical file in HDFS is gigabytes to terabytes in size. Thus, HDFS is tuned to support large files. It should provide high aggregate data bandwidth and scale to hundreds of nodes in a single cluster. It should support tens of millions of files in a single instance.

**Simple Coherency Model**\
HDFS applications need a write-once-read-many access model for files. A file once created, written, and closed need not be changed. This assumption simplifies data coherency issues and enables high throughput data access. A MapReduce application or a web crawler application fits perfectly with this model. There is a plan to support appending-writes to files in the future.

**Moving Computation is Cheaper than Moving Data**\
A computation requested by an application is much more efficient if it is executed near the data it operates on. This is especially true when the size of the data set is huge. This minimizes network congestion and increases the overall throughput of the system. The assumption is that it is often better to migrate the computation closer to where the data is located rather than moving the data to where the application is running. HDFS provides interfaces for applications to move themselves closer to where the data is located.

**Portability Across Heterogeneous Hardware and Software Platforms**\
HDFS has been designed to be easily portable from one platform to another. This facilitates widespread adoption of HDFS as a platform of choice for a large set of applications.


## What Hadoop File System (HDFS) is not suitable for

**Real time analytics**\
Hadoop works on batch processing, hence response time is high. *"The emphasis is on high throughput of data access rather than low latency of data access"*.

**Multiple smaller datasets**\
Hadoop framework is not recommended for small-structured datasets. This is not because Hadoop is slower on small datasets than on large datasets, but because for small data sets there are other tools available which can work on small datasets more efficiently.

**Multiple concurrent writers**\
Since HDFS makes the assumption that once a file is created, written, and closed, it need not be changed, having multiple concurrent writers to the files used by HDFS will introduce coherency issues.

**Updates to arbitrary parts of file**\
For the same reason as above, since HDFS assumes that what is once written will not be changed, data can only be appended, and cannot be placed at arbitrary places inside a written file.

## The HDFS architeture

HDFS can run on multiple cluters. Each cluster is a **name node**, and is tasked with managing the whole file system namespace. This basically means that it keeps track of where all the data is located. The name node keeps track of two kinds of meata data: The **FsImage** and the **EditLogs**.

The FsImage is a representation of the complete state of the file system name space state.

The EditLogs contain all changes made to the file system, which are not yet reflected in the most recent FsImage.

Each cluster consists of multiple nodes. These nodes are commodity hardware and are called **data nodes**. Each data node manages the storage of its own part of the cluster. HDFS follows a Master/Slave Architecture, where the NameNode is the master and the DataNodes are the slave nodes. At all times, the data resides in the data nodes, never in the name node. Whenever a change happens to data in the data nodes, these changes are logged in the name node. Also, the data nodes only contain data, not meta data.

There is also a node called the **secondary name node**. It is tasked with reading meta data from the ram of the name node and writing it to disk or the file system. It is also responsible for downloading the EditLogs from the name node and use it to update the name node's FsImage. In summary, the secondary name node regularly performs checkpoints in the HDFS, and because of this, it is also called the checkpoin node.

In HDFS, large data is split into blocks of 128MB. Blocks are the smallest unit in HDFS. Keeping the blocks this large ensures low access times.

After large files have been split into bloks and distributed across multiple machines in a cluster, the blocks are replicated three times, and distributed among other machines in the cluster. This makes HDFS fault tolerant, since any given block is backed up in case one (or three) machines in the cluster goes down.

The **client** is the interface through which the user, or an application used by the user communicates with HDFS. The client checks with the name node if the data nodes are ready to receive data. If they are, for each block, the client connects via TCP/IP to each the the corresponding datanodes and sends the IP address of the other datanodes in which the current block should be replicated. The first datanode in this pipeline connects to the others, and then starts pushing data.

## Repliication in HDFS
When a client is writing data to an HDFS file, its data is first written to a local file. Suppose the HDFS file has a replication factor of three. When the local file accumulates a full block of user data, the client retrieves a list of DataNodes from the NameNode. This list contains the DataNodes that will host a replica of that block. The client then flushes the data block to the first DataNode. The first DataNode starts receiving the data in small portions (4 KB), writes each portion to its local repository and transfers that portion to the second DataNode in the list. The second DataNode, in turn starts receiving each portion of the data block, writes that portion to its repository and then flushes that portion to the third DataNode. Finally, the third DataNode writes the data to its local repository. Thus, a DataNode can be receiving data from the previous one in the pipeline and at the same time forwarding data to the next one in the pipeline. Thus, the data is pipelined from one DataNode to the next.

Oh, and the elementary unit of data in HDFS is a a data block.

For reliability, all the file replicas are not placed on the same rack, but to avoid latency as a result of extensive communication between racks, the HDFS policy might place multiple replicas on the same rack, as long as more than ine rack is covered in total.

When reading data, HDFS will always attempt to read data from the replica which is closest to the reader.

On startup, the NameNode is in safe mode. While in this mode, no data blocks are replicated, but the NameNode checks whether each data node has at least the minimum number of replicas.

Data nodes send regular heart beats to the NameNode, and if the name node hasn't reeived a recent heartbeat from a DataNode, then it will consider the DataNode dead and no longer forward IO requests to them. In these cases new replication may be required. NameNodes never initiate remote procedure calls. It simply responds to requests from DataNodes or clients.

FsImage and EditLg may also be replicated, but doing this may degrade the performance.

## The NameNode
There is one name node per cluster. It keeps track of which data blocks are stored in which name node. Both clients and DataNodes can send communicate with the NameNode, but the NameNode never initiates remote procedure calls. In short, the NameNodes eep track of the state of the file system, by both keeping track of an FsImage and a LogFile. The NameNode does not need to store the FsImage persistently, as it can be recreated on startup from the DataNodes.

## Map functions
Mapper maps input key/value pairs to a set of intermediate key/value pairs.
The Mapper outputs are sorted and then partitioned per Reducer.
Input: (key, value)
Output: list(key, value)

## Reduce functions
Reducer reduces a set of intermediate values which share a key to a smaller set of values.
Input: (key, list(values))
Output: list(key, value)