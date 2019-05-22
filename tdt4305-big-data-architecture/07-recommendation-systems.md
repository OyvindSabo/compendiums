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

## Another example
The company you just started to work for is expert on recommender systems. Your own task is to develop good recommendation algorithms and methods.

**a) Focusing on pros and cons, compare ”collaborative filtering” against ”content-based recommendation”.**\

Collaborative filtering
| Pros                                 | Cons                                  |
|--------------------------------------|---------------------------------------|
|                                      | Have to ask people to give ratings. This migt give low effort reviews or require payment to be done properly       |
|                                      | Implicit collaborative filtering based on users' actions can show if something is popular before being tested, but does not give a good representation of actual satisfaction, and makes it difficult to give low ratings.                                                 |
|                                      | Cold start: Initially no ratings, so it's difficult to inspire people to contribute to a platform which is not fully up and running                                                                 |

Content-based recommendation
| Pros                                 | Cons                                  |
|--------------------------------------|---------------------------------------|
| Can give ratings to products wich                                               have never been rated                | Finding appropriate features can be                                             hard                                  ||                                      | Might require the user to create an                                             initial user profile of preferences,                                            or might risk to give bad                                                       recommendations in the beginning of                                             the user's user history               || Can avoid cold start problem by                                                 first asking user for what features                                             he/she likes                         | Never recommends things outside user's                                           profile                               |
| Able to recommend to users with                                                 unique tastes                        | Unable to utilize quality judgement                                             of other users                        |
| Able to recommend new and not yet                                               popular items                        | The greatness of items does not always                                          fully depend on easily measured                                                 features                              |
| Can provide information about why a                                             specific item is recommended         |                                       |

## Overlapping communities
A network is said to have community structure if the nodes of the network can be easily grouped into (potentially overlapping) sets of nodes such that each set of nodes is densely connected internally. In the particular case of non-overlapping community finding, this implies that the network divides naturally into groups of nodes with dense connections internally and sparser connections between groups. But overlapping communities are also allowed. The more general definition is based on the principle that pairs of nodes are more likely to be connected if they are both members of the same community(ies), and less likely to be connected if they do not share communities.

Two communities are overlapping if they share at least one node. Communities can be connected, even if they do not overlap, but in cases where communities have to be indentified from a social graph, it can be difficult to determine whether the communities overlap, or if they are just one large community. You might wonder how communities can be connected, but not overlap. For instance, one might say that a node is only a true member of a community if it is connected to more than one other node in that community. This means that two communities can be connected if no single node in any of the communities is connected to more than one node in the other community.

## Community detection
Community detection can be useful for:
- Recommender systems (Facebook can recommend new friends from the same community, YouTube can recommend videos liked by others who are in the same subscriber communities?, Advertising in general)

## How to find a community
To identify communities, one can use the Girvan-Newman algorithm. It requires that we are able to calculate the betweenness of all edges in the network. The betweenness is a measure of how important a specific edge is. The higher the betweenness, the higher the possibility that this edge ties communities together. In a connected graph, the betweenness is given as the sum of shortest paths going through an edge, if we find the shortest path between all pairs of nodes in the network. If one part of the network is connected to another part of the network with very few edges, these edges will have high betweenness, and in the Girvan-Newman algorithm, the network will be split here.

## Girvan-Newman algorithm
- The betweenness of all existing edges in the network is calculated first.
- The edge with the highest betweenness is removed.
- The betweenness of all edges affected by the removal is recalculated.
- Steps 2 and 3 are repeated until no edges remain.

As the algorithm progresses, the initail graph is split into smaller and smaller communities, but in the end, there are only individual nodes.

## The purpose of identifying communities
One can assume that people in the same community have somewhat alligning interests, which can be extrapolated or used for targeted advertising and recommendations of movies, Facebook-pages, friends, Linkedin contacts. Making recommendations based on identified communities will make the communities more strongly tied together. It can also be used for surveilance and statistics in general.

## Node centrality
Centrality aims to find the most important nodes in a network.
- **Degree Centrality:** The first and conceptually the simplest Centrality definition. This is the number of edges connected to a node. In the case of a directed graph, we can have 2 degree centrality measures. Inflow and Outflow Centrality
- **Closeness Centrality:** Of a node is the average length of the shortest path from the node to all other nodes
- **Betweenness Centrality:** Number of times a node is present in the shortest path between 2 other nodes

Node centrality can be used to identify individuals with a lot of influence, and can be used to identify hierarchy structures within criminal networks. Furthermore, node centrality can be used to assign a trust measure to nodes, which can help revent the development of fake profiles.