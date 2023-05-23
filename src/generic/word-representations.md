# Word Representations

Word representations are methods of creating a numerical representation of a span of text.

## Feature Sets

Feature sets are handcrafted lists of features, these features depend on the available information for the task. 

For example, common Named Entity Recognition features include:
- Word shapes
- Word features: suffixes, prefixes, capitalisation
- POS tags
- Word lookups (gazetteers)

They are expensive to create as the choice of features are manually chosen. This also makes them difficult to adapt and tune, as it is hard to tell what features are achieving what goals

# Embeddings

Vector embeddings are created automatically, learning representations of text based on a set of training data. 

Embeddings represent words as a point in a continuous multi-dimensional space

## Sparse Embeddings

Sparse embeddings are called as such as many of the entries in the vector will be zero. 

There are two methods of creating sparse embeddings

#### Term Frequency - Inverse Document Frequency (TF-IDF)

TF-IDF uses a term-document matrix, where each cell contains the count of a specific word (rows) in a specific document (columns). 

The matrix is weighted by two factors:
- Term frequency:  \\( \text{TF}(t, d) = \log_{10}(\text{count}(t,d)+1) \\)
- Inverse Document Frequency: \\( \text{IDF}(t)=\log_{10}(\frac{N}{\text{count}_{docs}(t)}) \\)

Which weights each cell by the number of times it appear in the document times the inverse of the number of documents it appears in. Giving the formula for weighting each cell as:

\\[ \text{TF-IDF}(t,d)=\text{TF}(t,d) \times \text{IDF}(t) \\]
 
Both factors are used as:
- Term frequency doesn't discriminate
- Inverse document frequency is useless alone, but shows which words are important to certain documents

#### Positive Pointwise Mutual Information (PPMI)

PPMI uses a term-term matrix, where each cell counts the co-occurrences of a target word (rows) and a context word (columns). Co-occurrences are defined as the number of times the context word appears within a ±N context window around the target word. 

PPMI is a measure of how much more two words co-occur than is expected if they were independent. 

It is calculated as:
\\[ \text{PPMI}(w, c) = \text{max}( \log_{2} \frac{P(w, c)}{P(w)P(c)}, 0) \\]

Only positive values are used as negative values are unreliable unless the corpus is massive. 

The probabilities are calculated: (w, c) is the cell count, (w) is the row count, and (c) is the column count, all divided by the total count

## Dense Embeddings

Dense embeddings are much smaller vectors (usually with dimensions in the hundreds), where all values take meaningful real numbers. 

Static embeddings learn one fixed embedding for each word.

Word2vec (Skip-gram with negative sampling) is an example algorithm to compute static dense embeddings. In which a self-supervised classifier is trained to classify is two words co-occur, i.e. the context word appears in a ±N window of the target word.  The weights are then used as the embeddings for words.

To train the weights:
- The classifier initialises two sets of weights randomly for each word, one for its target representation and the other for its context representation. 
- For all words, use the N context words and sample kN random words using weighted unigram frequency (i.e. counts to the power of \\(\alpha\\), commonly 0.75) to give more weight to rare words
- Adjust the weights to maximise the vector similarity of the context word pairs and minimise the similarity of the negative words

As two embeddings are learnt for each word it is common to either add them together or just use the target word embeddings

Other types of static include:
- *fasttext*: an extension on word2vec using a sub-word model to better handle unknown words and word sparsity
- GloVe: captures global corpus statistics from ratios of probabilities in a term-term matrix

Contextual embeddings (such as BERT) capture embeddings for each word sense. 

#### Benefits over sparse embeddings

Dense embeddings are better than sparse as:
- They require less weights due to the lower dimensions, which helps with generalisation and avoiding overfitting
- They capture relations between words

#### Semantic Properties of Dense Embeddings

In word2vec the size of the context window can alter the types of association between vectors:
- Smaller context windows (±2) show first-order co-occurrence (syntagmatic association). Which means the words are typically near each other. 
- Larger context windows (±5) show second-order co-occurrence (paradigmatic association). Which means the words share similar neighbours.

Embeddings encode properties such as:
- Relational similarity/meaning: e.g. King - man + women = queen (the parallelogram model)
- Implicit corpus bias

With multiple corpuses (e.g. historical, cultural, document type, ...) analysing the differences in word embeddings can show the change in meaning and associations words may have

