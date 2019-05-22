# Data streams

## Blom filter
A bloom filter is a space-efficient probabillistic data structure whihc is used to test whether an element is a member of a set. It might give false positives (it might say that the element is in the set, even if it isn't), but it never give false negatives (if it says that an element is not in a set, it is definitely not in the set). In other words, a query returns either "possibly in set" or "definitely not in set".

Bloom filters can for instance be used by web crawlers to efficiently find out whether a webpage has already been crawled, without comparing the webpage to all previously crawled webpages. It can also be used to filter out duplicate data in data analysis, or to prevent indexing duplicate mirror pages on the internet.

A bloom filter can be created by first mapping the relevant content to a binary number in some way or another. Then, this binary number is passed through multiple hash functions, typically involving a modulo operation.

Let's say we have a twitter post which produces this binary hash:\
10101011
Then we choose two hash functions. We let the first hash function be the number given by the odd numbered digits from the right mod 8, and the second hash function be the even numbered digits from the right mod 8 (assuming 1-indexing).

h1(10101011) = 0001 mod 1000 = 1 mod 8 = 1\
h2(10101011) = 1111 mod 1000 = 15 mod 8 = 7

Initially our bloom filter is 00000000.
We now swap in a 1 on the indexes corresponding to our two hashes (I'll do it from the right in this case).
Our bloom filter then becomes 01000001.

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
- **Standing queries:** Continuous query that is not only performed once, but is permanently installed. For example: "Continuously report the moving average of a specific stream."


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

## Another example
Suppose you work for a company analysing interest trends related to the new Sony A9 camera that Amazon just started to sel. Your company has decided to do this based on Social Media data, including Twitter and Facebook.

**a) Assume that to begin with, you decide to find the number of messages mentioning “Sony A9” by using so-called Standing Query. Explain the difference(s) between ”standing query”
and ”ad-hoc query”.**
- **Ad-hoc query:** A non-permanent query you ask at a about your data at a specific point in time. For instance, asking what the maximum value so far today is, is an ad-hoc-query. The output of an ad-hoq query is a value.
- **Standing query:** A query which continuously  answers questions about data, as it arrives. All ad-hoc queries can be standing queries, but the main difference is that staning queries output a stream of values, rather than just values.

**How can standing query solve your task?**\
Using a standing query enables us to track how the interest evolves over time. This can in turn be used to analyze how the interest covariates with advertising campaigns for instance.


b) Since we can consider social media data as streaming data, they have also other
characteristics and/or challenges than static data. Explain what these characteristics and/or challenges are.
**Streamig data characteristics:**
- Continuous processing, aggregation and analyzing data when created.
- Massive
- Unbounded
- Possibly infinite

**Streaming data challenges:**
- Not enough memory. The amount of data generated is too large to store in their original form for future analysis. Data reduced before being centralized.
- Can’t afford storing/revisiting the data. All the data to analyze is not available at the same time. Processing messages one at time.
- The structure of the data from different sources can be difficult to predict, making it difficult to use in real-time analyses.
- Since we're dealing with real-time data, system failures might lead to data loss.

**c) As an alternative approach to ”standing query”, you choose ”bit counting” to solve parts of the message counting task. Explain how this task can be translated to bit counting.**
Each value in the data stream can be translated to either 1 or 0 depending on whether it contains "Sony A9" or not respectively. This means that we have a way of mapping the stream to a bit stream on which we can use a standing bitcounting query to track popularity trends.

**d) Your analysis will see the trends the last 2 weeks and you consider number of “clicks” and “purchase” of products at Amazon directly as part of your analysis. Imagine that you want to calculate the fraction of the number of “click” and “purchase” on “Sony A9”. For this purpose we choose to use the sliding window principle. Assume that our sliding window has a size of 500 clicks and purchase in total (i.e., combined). Show how you can compute this fraction.**

Assuming that each input to the stream is either a click or a urchase and we have the following variables and constants
```
const wantedWindowLength = 500;
var window = new queue();
var input; // The value which enters the window
var output; // The value which leaves the window
var fraction;

while (stream.hasNext()) {
  input = stream.next() == 'buy' ? 1 : 0;
  output = window.peek() == 'buy' ? 1 : 0;
  fraction = fraction + input/window.size - output/window.size;
  emit(fraction);
  if (window.size === wantedWindowLength) {
    window.remove();
  }
  window.add(input);
}
```

**e) Speaking about clicks, explain how bloom filters could be useful when trying to find the number of clicks. Make any assumptions you find necessary.**\
If each piece of data is a set of a user, click and product, then the bloom filter can be used to filter out duplicate clicks from the same user.

**f) Use the bloom filter principle to fill out the table below:**\
| Stream element     | Hash function h1 | Hash function h2 | Filtered content |
|--------------------|------------------|------------------|------------------|
|                    |                  |                  | 00000000000      |
| 56 = 11 1000       |                  |                  |                  |
| 428 = 1 1010 1100  |                  |                  |                  |
| 875 = 11 0110 1011 |                  |                  |                  |
Use hi(x) = y1 mod 11 an h2(x) = y2 mod 11 as hash functions

| Stream element     | Hash function h1 | Hash function h2 | Filtered content |
|--------------------|------------------|------------------|------------------|
|                    |                  |                  | 00000000000      |
| 56 = 11 1000       | 4 mod 11 = 4     | 6 mod 11 = 6     | 00001010000      |
| 428 = 1 1010 1100  | 18 mod 11 = 7    | 14 mod 11 = 3    | 00011011000      |
| 875 = 11 0110 1011 | 25 mod 11 = 3    | 23 mod 11 = 1    | 01011011000      |
