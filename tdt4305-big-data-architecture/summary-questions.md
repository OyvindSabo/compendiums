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
The Storm architecture is shaped like a directed acyclic graph, where bolts can be used to split a single stream into multiple streams, or join multiple streams into a single stream. Unstructured, semi-structured and tructured data can be colected from different sources (spouts), and be combined, split and reduced before being centralized.
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
Data streams are:
- Continuous
- Massive
- Unbounded
- Possibly infinite
- Fast changing
2. **What are the biggest challenges with data streams?**\
The biggest challenges with data streams are as follows:
- It's impractical to store the whole streaming data.
- We have to be able to make critical calculations about the stream using a limited amount of (primary or secondary) memory.
- We can’t afford storing/revisiting the data, so we have to resort to single pass computation. 
3. **What two types of queries are relevant? Explain.**\
There are two types of queries relevant ot data streams:
- **Ad-hoc queries:** Normal queries asked one time about streams, for instance "What is the maximum value seen so far in stream S?"
- **Standing queries:** Queries that are, in principle, asked about the stream at all times, for instance "Report each new maximum value ever seen in stream S."
Note that ad-hoc queries and standing queries may very well be the same queries. THe main difference is that the output of an ad-hoc query is a value, while the output of a standing query is a stream of values.
4. **Explain examples of implementations utilizing data streams.**\
- **Collision avoidance systems:** Any application receiving constant sensor input hs to deal with data streams. Collission avoidance systems is an example of an application where being able to process such data in real-time is critical.
- **Financial markets:** Exchanges receive a constant stream of orders, and has to be able to process and output the resulting prices of those orders. Being able to have access to up-to-date data at all times is critical for the exchanges' reputation, and can be a matter of gain or loss for robot traders.
- **User analytics:** Online stores and social media constantly receives information about user activity. Being able to process these streams of data in real-time is important to be able to detect trends. For instance, the moment YouTube notices particular activity on a not yet highly rated movie, they may wish to suggest this video to more people to increase their user engagement.
5. **Explain how "Sliding Windows" can be used to handle data streams**\
When dealing with streaming data, the amount of data may be so large that we cannot afford to store data, or we might not have time to revisit it later, so we have to make critical calculations about the data as it arrives (single pass computation). To still be able to make valuable aggregations, we can use a sliding window. This means that rather than making computations on a whole data set, we make computations on data within a continuously updated time interval. We might for instance consider only the previous n elements received, where n is the window size. This can for instance be used to calculate moving averages.
6. **Explain what “Bit Counting” is. Use an example to support your explanation.**\
Bit counting is a technique where we map a data stream to a bit stream, that is, we map each value to 1 or 0 depending on whether or not it sataisfies a specific criteria. For a sliding window, this allows us to make very efficient computations about a stream within a given time interval by only considering the window's incoming and outgoing bits. For instance, we might have a stream representing the IP address of clients visiting a particular website. If we want to monitor traffic from a particular IP address, we can create a stream where all requests are mapped to 1 if they come from that particular address, and 0 otherwise. We can then use bitcounting for further analysis. THis can for instance be used to detect DOS attacks, or just monitoring traffic in general.

7. **Explain the DGIM algorithm. Explain how we can achieve a maximum of 50% error using this algorithm for bit counting.**\
The DGIM algorithm is an agorithm used to determine how many of the last bits are ones. The algorithm does not give an exact answer, but an estimate which is at most 50% of the window size off. The reason why we might want to use thi approximation rather than an exact method is that it uses only O(log<sub>2</sub>(n)) bits to represent a window of n bits. Rather than keeping track of all the ones in the window, te algorithm keeps track of two and two buckets of exponentially increasing sizes. Once a 1 enters the window, a bucket of size one is added. All new buckets are marked with the timestamp of the most recent bit in the bucket modulo the window size, which enables us to find out when a bucket is about to leave the window. If theere are already two buckets of size one, then the two oldest buckets of size one are merged together to form a bucket of size two. If this in turn leads to there being more than two buckets of size two, the two oldest buckets of size two are merged together to form a bucket of size four, and so on. To get an estimate of the number of ones in the window, we add together all the sizes of the the buckets whose time stamps are less than the window size into the past, except for the last one. Those are the buckets we know are definitely within the window. We do not know how many of the ones in the last bucket is still within the window, but averagely half of them are still within the window, so wealso add half the size of the oldest bucket to the count.

8. **Explain the principle behind “bucketing”.**\
buceting is...

9. **What are the ideas behind “bloom filter”?**\
The idea behind a bloom filter is to have an efficient probabillistic way of determining whether or not an element has been seen before without having to store all seen elements.\
**a. Explain the principle through examples.**\
When running a web crawler, which follows url links, we might want to be able to determine whether a url has been visited before or not, so that we can avoid indexing the same page multiple times. A bloom filter can efficiently tell us that we have visited a partiular url before (with the possibility of false positives), or that we have never seen a url before (guaranteed to be true).\
**b. What challenges are there?**\
Since a bloom filter of length n at most can keep track of 2<sup>n</sup> different hashes, the chance of the filter erronously telling us that a particular item has been seen before increases the longer we use the filter.\
**c. Can bloom filters be used for lookup? Justify your anwer.**\
Bloom filters can be used for lookup in the sense that we can use it to lookup whether or not it has registered a particular element at some point. By applying the same hash functions as was used to create the bloom filter, to the lookup element, we will get a bit string. If all the indexes containing ones in this bit string do also contain ones in the bloom filter, then the bloom filter might have registered the particular element, but if not, then we know for sure that the filter has not registered the element.

10. **What is sampling? Explain.**\
Sampling is to choose only a selection of a larger sense, with the assumption being that obervations made on the sample are also representative of the original set as a whole.

11. **What is distinct element counting?**\
Distinct element counting is a technique for estimating the number of distinct elements in a data set in cases where it is not feasible to store the whole data set for lookup.

12. **What is the Flajolet-Martin Approach? Explain.**\
The Flajolet-Martin approach estimates the number of distinct elements in a data set by means of single pass computation by hashing each input to a binary string, and maintaining a variable whose value is the longest series of sequential zeros at the end of the hash observed so far. The assumption here is that the more distinct elements, the higher the max count of sequential zeros at the end of the hashes. The algorithm results might, however, vary wildly, but one approach to counter this, is to use the Flajolet-Martin approach with multiple different hash functions and then take the average of the results.

### Questions - Recommender Systems (RS) I
1. **Explain the main idea and motivation behind recommender systems in general.**\
In short, the main goal of recommender systems is to recommend items to users to make users, content partners and websites happy.

With the evolution of the web and online retailers and content providers, the selection of items available from a single provider has moved from scarcity to abundance. With more items comes more choices, which in turn requires recommendation engines.

2. **Explain the "long tail"-phenomenon. What does this man for recommender systems in general?**\
The long tail phenomenon is, in business, at least, a trend which has developed as retailing has moved to the web. Previously, the main typical approach for retailers was to find out what products were the most popular, and choose to sell these few particular products in large volumes. As retatilers moved to the web, large online retailers, like amazon, realized that one could generate significantly more revenue by selling a larger selection of less popular, or more niche products in smaller volumes to a very much larger audience. The reason that the phenomenon is called the "long tail"-phenomenon is that if we plot a graph where the vertical axis is popularity, and the horzontal axis represents the items being sold, it becomes apparent that there is a long tail of products which are not considered popular which in total make up 80% of all sold items.

What this means for recommender systems is that the go-to approach of simply recommending what is averagely considered most popular will only target 20% of the potential market, which in turn means that the price of not using a sophisticated recommender system might be the loss of an 80% market share. Making good recommendations is not only about having a lot of data, but also about figuring out sophisticated algorithms to utilize this data in the best way possible.

3. **Set up the formal model for recommending, that is, what "utility function" and "utility matrix"?**\
The formal model of utility:
- **X** = Set of **Customers**
- **S** = Set of **items**
- **R** = Set of **ratings**
- **Utility function u:** **X** x **S** -> **R**\
The output from the utility function **u** is a utility matrix.

**3.1 What main challenges are we trying to solve?**\
Main challenges:
- We need to collect the data for the utility matrix.
- We need to be able to use the data we have to extrapolate the data we don't have. We're mainly interested in high unknown ratings, as we want to create recommendations, not warnings.
- We need to be able to measure the performance of the data we extrapolate to validate that the extrapolations we make are actually somewhat correct.

**3.2 In practice, how do we collect ratings? What is meant by implicit rating?**\
Ratings can either be collected explicitly or implicitly.
**Explicit:**
- Ask peple to rate items.
- Pay people to rate items.

**Implicit:**
- Learn ratings from user actions, e.g. purchase implies high rating.
- Difficult to implicitly determine low ratings.

**3.3 Why is it difficult to extrapolate the utility matrix?**\
- **Sparsity:** Most people have not rated most items, so there might simply not be enough data to extrapolate all the missing scores. Even if there is enough data to extrapolate the missing scores, there might not be enough data for the extrapolated results to be good.
- **Cold start:** Every time a new item or a new user arrives, these are without any history which can be used for extrapolation. 

4. **Explain the main principle behind content-based recommendation.**\
The main principle behind content-based recommendation is to recommend items which are similar to other items the user in question has rated highly. For instance, after a user has liked a picture on Instagram, Instagram might recommend images with similar tags, or images from the same author.\
**4.1 Explain how an item profile is created**\
An item profile is a vector of features. Depending on the item media, the item profile can either consist of metat data, or data from the item as well. For movies, it's natural to let the item profile consist of genre, actors, director, etc. For articles, the item profile might be a et of keywords from the text. To make sure to pick relevant keywords from the text, we can calculate tf-idf (term frequence * inverse document frequency) for all the words in the text and keep those with the highest score.

**4.2 How is content-based recommendation related to information retrieveval?**\
Since content-based recommendation is based on creating item profiles, and using these profiles to find other items with smilar profiles, it has much in common with information retrieval. Creating an item profile is much like indexing a ocument, or hashing it to a bucket of similar items.

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