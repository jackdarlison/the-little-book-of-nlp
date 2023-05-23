# Transformers

LSTMs and GRUs can handle long distance information, however, as they are recurrent in nature they cannot be parallelised, making them scale poorly with long sequence. 

Transformers are an architecture that can handle both long distance information and be parallelised, making them the best model for language processing.

In a transformer each input has access to all previous inputs, known as causal transformers (variations exist using different combinations of inputs), allowing the model to handle distance information. Most importantly, calculations performed for the current input input are independent of other inputs, making the system able to be parallelised. 

The core idea behind using other inputs to calculate the current input is the **self-attention**, which compares the current input to other inputs revealing their relevance in the current context. 

Transformers are contained in a modular block, taking an input sequence and outputting a sequence of the same length allowing them to be stacked. The transformer block consists of 4 layers:
1. Self-attention layer
2. Layer normalise
3. Feedforward layer
4. Layer normalise

Each layer feeds into the next layer. Residual connections are also added around the self-attention and feedforward layers (input is added to the output vector), which allows higher layers direct access to the information from lower layers. The normalisation keeps the values of the layer outputs and residual connections in a range that facilitates gradient based learning. 

Layer normalisation is implemented by subtracting the mean and dividing by the standard deviation, making a distribution with a mean of 0 and a standard deviation of 1. Gain and offset parameters are added after

## Self-attention

Self-attention is a more sophisticated variation of the attention used within encoder-decoder based models

In self-attention each input will play three different roles during the self-attention process:
- **Query**: the current focus (i.e. input) of attention being compared to the other inputs
- **Key**: an input being compared to the current focus
- **Value**: the value used to compute the output for the current focus of attention

To represent the roles a weight matrix will be created for each, which will be used to project the input \\( x_i \\) onto its representation of each role. \\[q_i = W^Qx_i;\ k_i = W^Kx_i;\ v_i = W^Vx_i \\]

Similar to encoder-decoder attention we need to calculate a relevance score for the current focus input \\( x_i \\) and each context input \\( x_j \\). However the scores are normalised by the dimensionality of the key (and query) vectors to avoid numerical issues

\\[ score(x_i, x_j) = \frac{q_i \cdot k_j}{\sqrt{d_k}} \\]

This is then softmax normalised and multiplied by \\( v_i \\)

Although, a each output can be computed independently there is no need to process each pair of inputs individually. We can instead represent the input as a matrix, and create matrices of the query, key, and value representations. This allows us to use efficient matrix multiplication implementations instead! 

\\[ Q = XW^Q;\ K=XW^K;\ V=XW^V \\]

Given these matrices we can neatly represent the process of self attention as a single function:

\\[ \text{SelfAttention}(Q, K, V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V \\]

As all inputs are being used as keys for all queries, if only preceding inputs should be used then the top-right triangle of the self-attention matrix should be set to negative infinity (which will become 0 with softmax)

#### Multi-head Attention

Different words in a sentence can relate to each other is in different ways simultaneously; it is difficult for a single self-attention layer to capture all these meanings. Multi-head attention solves this by using multiple parallel self-attentions layers at the same depth.

Each self attention layer is given its own set of query, key, and value matrices and undergoes self-attention in the same way. The output of each self-attention is then concatenated together and projected down to the original input dimension using another set of weights \\( W^O \\)

#### Positional Embeddings

So far transformers have had no notion of where each input is within the input sequence, unlike RNNs where this is built into the recurrent structure of the network. 

To achieve positional information we create a positional embedding for each word. This is achieved through  static functions which map integers to real values vectors such they retain positional relationships, the original transformer paper used a mixture of sine and cosine functions. This positional vectors share the same dimensions as the word embeddings and they are combined through addition. 

#### Transformers as Language Models

As autoregressive generation requires the calculation of the previous input, using transformers to generate text is not able to be made parallel. 

It is still possible to train the transformers in parallel as using teacher forcing allows all calculations to be independent. We can then use averaged cross entropy loss over the predicted outputs to adjust the weights. 

