# Recommedation systems

## Example

Imagine that we are hired in a new company which wants to specialize in streaming of movies. One of our tasks is to develop good recommendation algorithms and methods.

a) As part of our method we suggest giving the users the ability to rate movies, so that the system can use this to recommend movie in the future. Assume that the user has rated the following movier with three or more stars:
- Jurasic Park (Fantasi/SciFi)
- Harry Potter (Fantasi/Adventure)
- ET (SciFi)
- Lord of the Rings (Fantasi/Adventure)
- Alien (SciFi)
- Terminator (SciFi)
- 101 Dalmatians (Adventure/Family)
- Titanic (Romantic)
- Sleepless in Seattle (Romantic)
- Mr. Bean (Comedy)

**i. Explain how you would preceed to recommend the next movie for this user. Make the necessary assumptions.**\
The first challenge is to give the user enough incentive to actually bother rating the movie. We might pay users to rate movies, but introducing money will make people attempt to trick the system, which might lead to people giving movies they haven't seen fake ratings, so for now we will stick to the assumption that people feel the need to rate the movies they watch.

The general approach we want to take is to suggest a movie which is similar to ne that the user has already given a good rating. We might consider a movie as similar if it is in the same genre, has the same actors, or is rated well by someone who also gave a good rating to movies the user has given a good rating.

More specifically, we want to create item profiles for each movie. An item profile is a set of features described by keywords. We then give each movie a score based on term frequency multiplied by the inverse document frequency.

**ii. Would it be best to use a content-based” recommendation method or collaborative filtering?**\
Considering that it's a new company, if they make themselves dependent on collaborative filtering, they will most likely face cold-start and sparsity problems. By rather using the content-based approach, they avoid the no first rater problem, and they can rather make the user suggest what he or she likes upon first using the site. In this case the user has incentiv to share his or her opinion.

b) Assume the following user rating table:

                               users
|   | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| 1 | 1 |   | 3 |   |   | 5 |   |   |
| 2 |   |   | 5 | 4 |   |   | 4 |   |
| 3 | 2 | 4 |   | 1 | 2 |   | 3 |   |
| 4 |   | 2 | 4 |   | 5 |   |   | 4 |
| 5 |   |   | 4 | 3 | 4 | 2 |   |   |
| 6 | 1 |   | 3 |   | 3 |   | A | 2 |
movies

Use the item-item collaborative filtering method to suggest user nr. 7's rating of movie nr. 6, which is to say: What is the rating value of A?

Estimate rating of movie 6 by user 

We calculate the mean rating for each movie (row)
We subtract the mean from row 6 and get [-1.25, 0, 0.75, 0, 0.75, 0, 0, -0.25]

                               users
|   | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | Average rating       |
|---|---|---|---|---|---|---|---|---|----------------------|
| 1 | 1 |   | 3 |   |   | 5 |   |   | (1+3+5)/3     = 3.00 |
| 2 |   |   | 5 | 4 |   |   | 4 |   | (5+4+4)/3     = 4.33 |
| 3 | 2 | 4 |   | 1 | 2 |   | 3 |   | (2+4+1+2+3)/5 = 2.40 |
| 4 |   | 2 | 4 |   | 5 |   |   | 4 | (2+4+5+4)/4   = 3.75 |
| 5 |   |   | 4 | 3 | 4 | 2 |   |   | (4+3+4+2)/4   = 3.25 |
| 6 | 1 |   | 3 |   | 3 |   | A | 2 | (1+3+3+2)/4   = 2.25 |
movies

We subtract the mean movie rating from each corresponding movie score and use the Pearsson correlation coefficient to calculate the similarity between row 6 and the other rows.

                               users
|   | 1     | 2     | 3     | 4     | 5     | 6     | 7     | 8     | sim(row 6, row i) |
|---|-------|-------|-------|-------|-------|-------|-------|-------|-------------------|
| 1 | -2.00 | 0     | 0.00  | 0     | 0     | 2.00  | 0     | 0     | 0.53              |
| 2 | 0     | 0     | 0.67  | -0.33 | 0     | 0     | -0.33 | 0     | 0.37              |
| 3 | -0.40 | 1.60  | 0     | -1.40 | -0.40 | 0     | 0.60  | 0     | 0.05              |
| 4 | 0     | -1.75 | 0.25  | 0     | 1.25  | 0     | 0     | 0.25  | 0.29              |
| 5 | 0     | 0     | 0.75  | -0.25 | 0.75  | -1.25 | 0     | 0     | 0.41              |
| 6 | -1.25 | 0     | 0.75  | 0     | 0.75  | 0     | 0 (A) | -0.25 | 1.00              |
movies

calculation for row 5 = (0.75\*0.75+0.75\*0.75) / sqrt((-1.25)²+0.75²+0.75²+(-0.25)²)*(0.75²+(-0.25)²+0.75²+(-1.25)²))

The way sim works is that row 6 = row i * sim (row i, row 6)

We calculate the expected score for A by using the weighted average of the expected scores:
(0.37\*4 + 0.05\*3) / (0.37+0.05) = 3.88
