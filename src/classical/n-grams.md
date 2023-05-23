# N-Grams 

A language model is one that assigns a probability to each possible next word in a sequence. 

The probability of a sequence of words is the joint probability of each word given all previous words.

\\[ P(w_{1:n}) = P(w_1)P(w_2 \vert w_1)P(w_3 \vert w_{1:2})...P(w_n \vert w_{1:n-1}) \\]

As there is no way to compute the probability of a word given a long sequence of preceding words, n-grams use a generalised form of the Markov assumption (the current state only depends on the previous, i.e. the bigram) where the current state depends on the previous n-1 states. 

For a bigram model:

\\[ P(w_{1:n}) = P(w_1)P(w_2 \vert w_1) \dots P(w_n \vert w_{n-1}) \\]

Where there is a start if sentence marker:

\\[ P(w_{1:n}) = P(w_1| \lt s \gt)P(w_2 \vert w_1) \dots P(w_n \vert w_{n-1}) \\]

The probability (maximum likelihood estimation, MLE) for an n-gram is given by the count of the n-gram normalised by the count of the preceding (n-1)-gram:

\\[ P(w_n | w_{n-N+1:n-1}) = \frac{C(w_{n-N+1:n-1}w_n)}{C(w_{n-N+1:n-1})} \\]

Where \\( N \\) is the size of the n-gram

To evaluate an n-gram model [perplexity](../generic/metrics.md#Perplexity) is used.

## Dealing with Unknown Words

It is possible that words may appear in the test set that where not in the training set. 

One method of dealing with this is to enforce a *close vocabulary*, where all test words need to be known.

However, in most situations language models need to be able to handle unknown words, also known as *out of vocabulary* (OOV) words.  To do this a new pseudo-word token \<UNK\> is added to the vocabulary, making it an *open vocabulary*

There are two common ways to create an open vocabulary:
- Choose a fixed vocabulary and replace all words in the training set with the \<UNK\> that do not appear in the fixed vocabulary
- Replace all words in the training set that have less than a certain frequency or are not in the most frequent X words with \<UNK\>

This pseudo-word is then estimated like any other word. Unknown words in the test set are then treated as the \<UNK\> token

## Dealing with Sparse Data

Another issue that occurs in n-gram models is the sparsity of n-grams, i.e. known words appearing in a new sequence or context.

#### Laplace Smoothing

One method of solving this is shifting some of the probability mass from probable words to words that appear less. This is known as **Laplace** smoothing where \\( k \\) is added to all all word counts, this is commonly known as add-1 or add-\\(k\\) smoothing. 

\\[ P_{\text{Laplace}}(w_n \vert w_{n-N+1:n-1}) = \frac{C(w_{n-N+1:n-1}w_n)+k}{C(w_{n-N+1:n-1})+kV} \\]

Note: \\( kV \\) has been added to the denominator to take into account the extra counts across the whole vocabulary

#### Backoff and Interpolation

Another method of dealing with sparse n-grams is to take into account the probability of the lower-order n-grams. There are two methods of doing this.

**Backoff** is one method, which uses the highest-order n-gram that has been seen.

In order for backoff to give a valid probability distribution, a function \\( \alpha \\) is used to distribute the mass to the lower-order n-grams. This is known as **Backoff with discounting** or **Katz Backoff**

**Interpolation** is another method which mixes the probabilities of the n-gram and its lower-order variations. This is done by adding the probability of each n-gram together, each weighted by some factor \\( \lambda_i \\). The sum of all weights needs to add to 1 to ensure the probability distribution remains valid. 

\\[ \hat{P}(w_n | w_{n-2:n-1}) = \lambda_1P(w_n) + \lambda_2P(w_n|w_{n-1}) + \lambda_3P(w_n|w_{n-2:n-1}) \\]

It is possible to also compute weights depending on the context sequences. 

These weights are trained using a held-out corpus, ensuring it does not overfit to the actual training data. 

Other forms of dealing with unknown contexts are:
- Stupid backoff
- Kesner-Ney Smoothing