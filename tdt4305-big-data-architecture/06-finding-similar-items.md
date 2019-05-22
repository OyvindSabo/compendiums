# Finding similar items

## Shingling
- Shingles represent documents as sets.
- A k-shingle for a document is any sequence of k tokens that appear in the document
- Tokens can be characters, words, or something else, depending on the application
- Each document can be represented with the set of k-shingles that appear in it

### Example of shingling
Question: What is the 3-shingle of "the colour of the sky is blue"?\
Answer: { "the colour of", "colour of the", "of the sky", "the sky is", "sky is blue" }

### How large should k be?
k should be large enough so that:
- The probability of two documents having the same shingle by chance is low.
- Two similar documents contain at least one matching shingle

### Hashing
There are (number of characters)<sup>k</sup> possible shingles, so to not have to store all possible shingles as keys, we hash the shingles.

## Locality sensitive hashing
Locality-sensitive hashing (LSH) is an algorithmic technique that hashes similar input items into the same "buckets" with high probability. The number of buckets are much smaller than the universe of possible input items. Since similar items end up in the same buckets, this technique can be used for data clustering and nearest neighbor search. It differs from conventional hashing techniques in that hash collisions are maximized, not minimized. Alternatively, the technique can be seen as a way to reduce the dimensionality of high-dimensional data; high-dimensional input items can be reduced to low-dimensional versions while preserving relative distances between items.

When doing locality sensitive hashing, we typically want the input to be a document, and the output to be a hash value, for instance a binary sequence of a given length.

## MinHashing
MinHashing is used to create small signatures of sets, which in turn can be used to determine equality between sets.

The min hash is found by a binary number where the nth digit from the left represents whether or not the set contains shingle nuber n.

The result is hashed a number of times by first permutating the rows randomly. Then, element x in row y of the hash is given by the index of the first row from the top in the permutated result which contains a 1 in column x.

### Example of MinHashing
| lx | Element | S1 | S2 | S3 | S4 | lx2 |
|----|---------|----|----|----|----|-----|
| 0  | p       | 0  | 0  | 0  | 1  | 3   |
| 1  | a       | 0  | 1  | 1  | 1  | 2   |
| 2  | g       | 1  | 1  | 0  | 0  | 1   |
| 3  | i       | 1  | 1  | 1  | 1  | 0   |
| 4  | k       | 0  | 1  | 0  | 0  | 4   |

The figure above shows the occurrence matrix for the elements p/a/g/i/k in the sets S1/S2/S3/S4. What are the MinHash signatures given the permutations lx and lx2?

**min(lx):**
| lx | Element | S1 | S2 | S3 | S4 |
|----|---------|----|----|----|----|
| 0  | p       | 2  | 1  | 1  | 0  |
How did we get the above result? With permutation lx (as in the occurrence matrix further up), The first occurrence of 1 in column S1 is on row 2, the first occurrence of 1 in column s2 is on row 1, and so on.

| lx2 | Element | S1 | S2 | S3 | S4 | lx2 |
|-----|---------|----|----|----|----|-----|
| 3   | i       | 1  | 1  | 1  | 1  | 0   |
| 2   | g       | 1  | 1  | 0  | 0  | 1   |
| 1   | a       | 0  | 1  | 1  | 1  | 2   |
| 0   | p       | 0  | 0  | 0  | 1  | 3   |
| 4   | k       | 0  | 1  | 0  | 0  | 4   |

**min(lx2)_**
| lx | Element | S1 | S2 | S3 | S4 |
|----|---------|----|----|----|----|
| 0  | p       | 0  | 0  | 0  | 0  |

## Locality-Sensitive Hashing
If we use sampling (for instance evaluating every 10th element) rather than evaluating all input data, we will not get any good indication on whether an input matches previus input data. In this case where it requires too much time to hash all input data and evaluate it agains a filter, a better evaluation approach would be to find candidate pairs (potentially equal documents) before actually using a lot of computational force to compare them. We do this by mapping the documents to buckets, and if two documents are in the same bucket, those documents are candidate pairs.
