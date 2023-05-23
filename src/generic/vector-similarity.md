# Vector Similarity

Measuring the similarity of two vectors is an important topic as many NLP applications use vector representations of words, i.e. embeddings

A simple method of measuring how similar two vectors are is the dot product:

\\[ v \cdot w = \sum^N_{i=1} v_iw_i \\]

However, this is method is biased towards longer vectors. **Cosine similarity** or the normalised dot product is commonly used instead:

\\[ \text{cosine}(\theta) = \frac{a \cdot b}{|a||b|} \\]

Where vectors are pre-normalised these metrics are interchangeable.

