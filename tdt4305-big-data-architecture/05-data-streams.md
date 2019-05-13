# Data streams

## Example

Assume that we are going to analyze how many times a thema about the American election is mentioned in messages on social media like Twitter.

**a) Discuss the characteristics and/or challenges of data streams.**\
We deal with data streams when we need to do continuous processing, aggregation and analyzing data at the time of its creation.

Situations were one might need to operate on data streams:
- Monitoring trends
- Communicate
- Recommendations
- Searching

Challenges with dealing with data streams:
- Data arrival rate might be higher than data process rate (challenging to scale).
- It is impractical to store the whole streaming data.
- How to make critical calculations about the stream using a limited amount of memory.
- It can be difficult to handle unstructured data in real-time.
- Since we're working on real-time data, soft or hard failures can cause loss of data, since we cannot afford storing/revisiting the data.

**Mention two other examples where one has to deal with data streams (in addition to Twitter and social media in general).**
- Prcessing live data from stock markets
- Processing live data from sensors


b) We distribute between two types of queries for data streams. Explain what they are, and provide examples.
- **Ad-hoc queries:** Normal queries asked one time about streams. For example: What is the maximum value of a particular stream so far?"
- **Standing queries:** Continuous query that is not only performed once, but is permanently installed. For example: "Continuously report the moving averag of a specific stream."


**c) Assume that we want to find the fraction of Twitter messgages relating to the election over a given time period. To do this, we use a sliding window of 1000 Tweets. How would we go about calculating this fraction?**\
We have a counter with an initial value of 0. As we fill up the window,we increase the counter by 1/1000 for each tweet containing words relating t the election. Once the window has filled up, for incoming tweet, we continue adding 1/1000 to the counter if the tweet relates to the election, and for each outgoing Tweet, we subtract 1/1000 from the counter if the tweet contains words related to the election.

**d) Can the problem described above be seen as a variant of "bit counting"?**/
Yes, the fraction of the tweets relating to the election is simply the answer of the bit counting problem divided by the constant size of the sliding window.

**e) Use the “bloom filter”-prinsciple to fill out the table below**
| Strømelement       | Hash-funksjon - h 1 | Hash-funksjon - h 2 | Filtrere Innhold |
|--------------------|---------------------|---------------------|------------------|
|                    |                     |                     | 00000000000      |
| 39 = 10 0111       |                     |                     |                  |
| 214 = 1101 0110    |                     |                     |                  |
| 353 = 01 0110 0001 |                     |                     |                  |

Hint: use h(x) = y mod 11, where y is taken from  odd bits from the right of x, or even bits from the right of x respectively.

| Strømelement       | Hash-funksjon - h 1 | Hash-funksjon - h 2 | Filtrere Innhold |
|--------------------|---------------------|---------------------|------------------|
|                    |                     |                     | 00000000000      |
| 39 = 10 0111       | 3                   | 5                   | 00010100000      |
| 214 = 1101 0110    | 14 mod 11 = 3       | 9                   | 00010100010      |
| 353 = 01 0110 0001 | 25 mod 11 = 3       | 4                   | 00011100010      |

**f) Assume that we want to analyze the last 11 incomign messages. On twitter, a lot of messages will be sent multiple times or retweeted. Explain how we can use bloom filters to filter away such duplicate messages.**\
Bloom filters are typically used to find out whether an entry already has been registered. Duplicate messages will, if completely unaltered, produce the same output, which, which is added to the bloom filter. After the first of many duplicate entries have passed through the bloom filter, the bloom filter will be able recognize new content providing the same output from the has functions as content it has already seen before. This assumes that we only look at the actual content of the Tweets and not include any meta data, as this most likely would result in different hash outputs.

**g) Let's assume that when the 11 tweets have arrived, we have a stream of data which looks like this:**\
**10100101010***\
**Can we have seen the message represented by y = 1111011 before?**\
h<sub>1</sub> yields 13 mod 11 = 2.\
h<sub>2</sub> yields 7.\
Together, this produces the output 00100001000\
For every position of this output containing 1, there is a 1 on the corresponing position in the data stream, so we can have seen the message represented by 1111011 before.

