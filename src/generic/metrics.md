# Evaluation Metrics



## ROUGE

ROUGE is used to evaluate text summarisation.

It evaluates a summary by the proportion on n-grams in the summary that also appear in a reference summary

ROUGE is recall oriented so depended on the quantity of matches not the quality of them.

## BLEU

BLEU is used to evaluate machine translations.

It evaluates a generated text against a reference text by comparing the number of n-grams that appear in the generated that also appear in the reference text

BLEU is purely precision based (how much of the translation is good) and ignores recall factors (how much did it translate)

## Perplexity

Perplexity is used to evaluate language models, i.e. models which assign a probability distribution for the next word in a sequence. 

It measures how good a vocabulary is it predicting a target text. It is computed as the probability of the words in the text appearing in that order, which is then inverted and normalised by the number of words. 

Perplexity is given as:
\\[ Perplexity(w_1...w_N)=\sqrt[N]{\frac{1}{P(w_1w_2...w_N)}} \\]

Generalising to a bigram model gives:
\\[ Perplexity(w_1...w_N)=\sqrt[N]{ \prod_{i=1}^{N} \frac {1} {P(w_i \vert  w_{i-1})} } \\]
Note: \\( N \\) is the number of bigrams not words 

## Precision, Recall, and F1

Precision, Recall, and F1 are used to evaluate classification tasks.

Precision is defined as the proportion of predicted items that are actually correct compared to all items predicted to be correct:
\\[ precision=\frac{TP}{TP+FP} \\]

Recall is defined as the proportion of predicted items that are actually correct compared to the actual set of correct items
\\[ recall=\frac{TP}{TP+FN} \\]

There is a trade off between these two metrics so F1 score is used instead, which incorporates both metrics. 
\\[ F1=\frac{2 \times precision \times recall}{precision+recall} \\]

To adapt these to multi-class problems there are two methods:
- macro-averaging: compute the performance for each class individually, then average over the classes
- micro-averaging: pool the decisions for each class into a single binary confusion matrix, then compute precision and recall

## Cross Entropy Loss

