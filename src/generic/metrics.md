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

The probability of a sequence of words using the Markov assumption (bigram model) is:

\\[ P(w_{1:n}) = P(w_1)P(w_2 \vert w_1) \dots P(w_n \vert w_{n-1}) \\]

Where there is a start if sentence marker:

\\[ P(w_{1:n}) = P(w_1| \lt s \gt)P(w_2 \vert w_1) \dots P(w_n \vert w_{n-1}) \\]

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

Cross entropy loss (negative log likelihood loss) measures how close an estimated output \\(\hat{\ y} \\) is to the correct output \\( y \\). We aim to minimise the negative log probability of the true \\( y \\) labels in the training data. Cross entropy loss is given as:

\\[ L_{CE}(\hat{\ y}, y) = -( y \log \hat{\ y} + (1 - y) \log (1 - \hat{\ y}) )\\]

When the classifier is binary, correct output being 0 or 1, the cross entropy loss is just \\(\ -\log\hat{\ y} \\)

For multi-class output *Categorical Cross Entropy Loss* is given:

\\[ L_{CCE}(\hat{\ y},y) = -[\sum_{i=1}^{N} y_i * log(\hat{y_i})] \\]

To find the loss of a sequence of outputs, we average the cross entropy loss over all the output states:

\\[ L = \frac{1}{T}\sum^T_{i=1}L_{CCE}(\hat{y_i}, y_i) \\]