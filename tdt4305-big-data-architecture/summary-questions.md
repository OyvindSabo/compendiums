# Summary questions

### Questions - Streaming data systems
1. **Explain the motivation behind the development of systems for streaming data**\
Large enterprises have to deal with data sets which are continuous, massive, unbounded and possibly infinite. The data changes quickly and requires fast real-time responses. Being able to process this data can be a matter of life and death in collision detection, or a matter of financial loss in the case of downtime, or hung up networks.

In these systems input tuples enter at a rapid rate, at one or more input ports. It's impractical to store the whole streaming data, so we're faced with the challenge of making critical calculations about the stream using a limited amount of (primary or secondary) memory. Since we cannot afford to store or revisit the data, we have to resort to single pass computations, making calculations based on incoming data as it arrives.

Furthermore, external memory algorithms for handling data sets larger than main memory cannot be used since they do not support continuous queries and provide too slow real-time responses.

2. **Give typical examples of streaming data that the systems should be able to handle in a satisfactory manner.**\
- Query streams e.g. what queries are more frequent today than yesterday?
- Click streams e.g. what pages have gotten an unusual number of hits in the past hour (can be used to detect annoyed users getting stuck in hard-to-use interfaces).
- IP packets at switch (can be used to detect DOS attacks, or be used to optimalize routing).

3. **What is meant by "delivery semantics"? Explain**\
Delivery semantics describe message guarantees. The different kinds of message guarantees are as follows:
- **At most once:** Messages may be lost, but never redelivered.
- **At least once:** Messages will never be lost and may be redelivered.
- **Exactly once:** Messages are never lost and never redelivered, perfect message delivery.

4. **Explain the differences between soft and hard failure in regards to "fault-tolearance"**\
- **Soft Failure** is the inability to pre-process or ingest a received record either due to an unexpected (null) value, duplicate record or due to inherent bug(s) in user-defined functions (UDF).
- **Hard Failure** is the inability to support dataflow due to loss of one or more participant nodes.

5. **Explain the main principle of "Storm".**\
**5.1. What components/modules does Storm consist of?**\
Storm consists of streams, spouts and bolts.
- A **Stream** is an unbounded sequence of tuples.
- A **Spout** is the source of a Stream, for instance reading from the Twitter streaming API .
- A **Bolt** processes input streams and produces new streams, for instance by applying functions, filters, aggregations, or joins.

**5.2. Show how the Storm architecture looks**\
The Storm architecture is shaped like a directed acyclic graph, where bolts can be used to split a single stream into multiple streams, or join multiple streams into a single stream.
Spout --> Stream of tuples --> Bolt --> Stream of tuples --> Bolt

6. **Explain the main differences between Storm and Spark**\
|                     | Storm                              | Spark                  |
|---------------------|------------------------------------|------------------------|
| Processing model    | Event-streaming                    | Micro-Batching / Batch |
| Delivery guarantees | At most once / at least once       | Exactly once           |
| Latency             | Sub-second                         | Seconds                |
| Language options    | Java, Clojure, Scala, Python, Ruby | Java, Scala, Python    |

7. **What are the main ideas behind AsterixDB?**\
The slogan of AsterixDB is **"One Size Fits a Bunch"**, and is defined by the following capabilities:
- Flexible NoSQL data model
- Scalable storage & indexing
- Continuous query support
- Windowed aggregation
- Full declarative query capability
- Web	data types & Search I
- Fast continuous data ingestion
- Web	data types & search I

8. **What is the main advantage with AsterixDB feeds compared to Storm and Spark?**\
AsterixDB differs from Storm and Spark in the following ways:
- **AsterixDB** is scalable, fault tolerant, and facilitates elastic data ingestion.
- **AsterixDB** has a plug-and-play model to cater to a wide variety of data sources. Storm is limited to process streaming data, and Spark is limited to batch processing.
- **AsterixDB** is a custom built solution, and does not depend on multiple modules working together. This means that it is both more efficient and easier to use than for e.g. Storm + MongoDB.
- **AsterixDB** has can handle big data with fewer moving parts than Storm and Spark.

9. **Explain different **indigestion** policies with AsterixDB Feeds.**\
In AsterixDB, the user can form policies to decide what will happen with data in the case of data indigestion. Data indigestion happens when the data is subject to expensive pre-processing prior to persistence, or the data arrival rate is higher than the rate at which it acn be pre-processed. The different indigestion policies are as follows:
- **Basic:** Buffer excess records in memory for deferred processing.
- **Spill:** Spill excess records to disk for deferred processing.
- **Discard:** Discard excess records altogether.
- **Throttle:** Randomly filter out records to regulate the rate of arrival.
- **Elastic:** Scale out/in to adapt to the rate of arrival.

### Questions - Mining streaming data
1. **Explain the characteristics of a data stream (especially compared to static data).**\
TODO
2. **What are the biggest challenges with data streams?**\
TODO
3. **What two types of queries are relevant? Explain.**\
TODO
4. **Explain examples of implementations utilizing data streams.**\
TODO
5. **Explain how "Sliding Windows" can be used to handle data streams**\
TODO
6. **Explain what “Bit Counting” is. Use an example to support your explanation.**\
TODO
7. **Explain the DGIM algorithm. Explain how we can achieve a maximum of 50% error using this algorithm for bit counting.**\
TODO
8. **Explain the principle behind “bucketing”.**\
TODO
9. **What are the ideas behind “bloom filter”?**\
**a. Explain the principle through examples.**\
TODO\
**b. What challenges are there?**\
TODO\
**c. Can bloom filters be used for lookup? Justify your anwer.**\
TODO
10. **What is sampling? Explain.**\
TODO
11. **What is distinct element counting?**\
TODO
12. **What is the Flajolet-Martin Approach? Explain.**\

### Questions - Recommender Systems (RS) I
1. **Explain the main idea and motivation behind recommender systems in general.**\
TODO
2. **Explain the "long tail"-phenomenon. What does this man for recommender systems in general?**\
TODO
3. **Set up the formal model for recommending, that is, what "utility function" and "utility matrix"?**\
TODO\
**3.1 What main challenges are we trying to solve?**\
TODO\
**3.2 In practice, how do we collect ratings? What is meant by implicit rating?**\
TODO\
**3.3 Why is it difficult to extrapolate the utility matrix?**\
TODO
4. **Explain the main principle behind content-based recommendation.**\
TODO\
**4.1 Explain how an item profile is created**\
TODO\
**4.2 How is content-based recommendation related to information retrieveval?**\
TODO\
**4.3 Exlain how one can predict the user profile.**\
TODO\
**4.4 Discuss advantages and weaknesses with content-based recommendation.**\
TODO
5. **Explain the main principle behind recommendation based on "Collaborative Filtering” (CF).**\
TODO\
**5.1 What is the main distinction from content-based recommendation? Explain.**\
TODO
6. **CF (collaborative filtering) is based on other users' "ratings". Explain three sample methods which can be used for this.**\
TODO
**6.1 Explain how one might predict the right product to the right user, that is, how does on go from similarity to recommendation?**\
TODO\
**6.2 Explain what challenges each of these methods entail.**\
TODO
7. **What is the main difference between user-to-user filtering and item-to-item filtering?**\
TODO
8. **Discuss advantages and weaknesses with CF (collaborative filtering)**\
TODO

### Questions - Social Network
1. **Wy do we need to analyze social grpahs?**\
TODO\
**a. What are social graphs?**\
TODO\
**b. Give examples of social graphs.**\
TODO
2. **Explain the difference between overlapping and non-overlapping communities in graphs.**\
TODO\
**a. How can we simply find out if to graphs overlap?**\
TODO
3. **What is the main purposes of community detection? Explain.**\
TODO
4. **What is meant by the term “edge betweenness”. Explain.**\
TODO
5. **Explain with an example the method behind the Girvan-Newman algorithm.**\
TODO
6. **What is the spectral clustering method?**\
TODO