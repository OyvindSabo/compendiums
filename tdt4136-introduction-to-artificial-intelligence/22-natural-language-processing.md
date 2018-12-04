# Natural Language Processing

## 22.2 Text Classification

### Challenges when performing sentiment analysis
- Figurative language can change the meaning (sarcasm, irony, humour)
- Modifers (e.g., negation) can change or reverse the meaning.
- The very restricted length of the messages.
- Word meaning can be domain dependent.
- References and quotes to other texts.
- Non-conventional grammar.
- Idiomatic language.
- Mixeing languages.
- Spelling errors.
- Hashtags.
- Brevity.

## 22.3.1 Information Retrieval (IR) Scoring Functions

### Term frequency
The number of times a term occurs in a document is called its term frequency.

### Inverse document frequency
Because the term "the" is so common, term frequency will tend to incorrectly emphasize documents which happen to use the word "the" more frequently, without giving enough weight to the more meaningful terms "brown" and "cow". The term "the" is not a good keyword to distinguish relevant and non-relevant documents and terms, unlike the less-common words "brown" and "cow". Hence an inverse document frequency factor is incorporated which diminishes the weight of terms that occur very frequently in the document set and increases the weight of terms that occur rarely. The specificity of a term can be quantified as an inverse function of the number of documents in which it occurs.