# Finding similar items

## Shingling
- Shingles represent documents as sets.
- A k-shingle for a document is any sequence of k tokens that appear in the document
- Tokens can be characters, words, or something else, depending on the application
- Each document can be represented with the set of k-shingles that appear in it

### Example of shingling
Question: What is the 3-shingle of "the colour of the sky is blue"?\
Answer: { "the colour of", "colour of the", "of the sky", "the sky is", "sky is blue" }

### How large shoulf k be?
k should be large enough so that:
- The probability of two documents having the same
shingle by chance is low.
- Two similar documents contain at least one matching shingle

### Hashing
There are (number of characters)<sup>k</sup> possible shingles, so to not have to store all possible shingles as keys, we hash the shingles.

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

**min(lx):**\
| lx | Element | S1 | S2 | S3 | S4 |
|----|---------|----|----|----|----|
| 0  | p       | 2  | 1  | 1  | 0  |

| lx2 | Element | S1 | S2 | S3 | S4 | lx2 |
|-----|---------|----|----|----|----|-----|
| 3   | i       | 1  | 1  | 1  | 1  | 0   |
| 2   | g       | 1  | 1  | 0  | 0  | 1   |
| 1   | a       | 0  | 1  | 1  | 1  | 2   |
| 0   | p       | 0  | 0  | 0  | 1  | 3   |
| 4   | k       | 0  | 1  | 0  | 0  | 4   |

**min(lx2)_**\
| lx | Element | S1 | S2 | S3 | S4 |
|----|---------|----|----|----|----|
| 0  | p       | 0  | 0  | 0  | 0  |