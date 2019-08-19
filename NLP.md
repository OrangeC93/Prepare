# NLP
1. Context-free models such as word2vec or GloVe generate a single word embedding representation for each word in the vocabulary. For example, the word “bank” would have the same context-free representation in “bank account” and “bank of the river.”
2. Contextual models instead generate a representation of each word that is based on the other words in the sentence. Contextual representations can further be unidirectional or bidirectional. For example, in the sentence “I accessed the bank account,” a unidirectional contextual model would represent “bank” based on “I accessed the” but not “account.” However, BERT represents “bank” using both its previous and next context — “I accessed the … account” — starting from the very bottom of a deep neural network, making it deeply bidirectional.

# Traditional Embedding
## BoW
Each word or n-gram is linked to a vector index and marked as 0 or 1 depending on whether it occurs in a given document.
- don’t encode any information with regards to the meaning of a given word.
- word occurrences are evenly weighted independently of how frequently or what context they occur. 

## TF-IDF
Numerical statistic that is intended to reflect how important a word or n-gram is to a document in a collection or corpus. 
- still unable to capture the word meaning.

## Distributional Embeddings
Enable word vectors to encapsulate contextual context.

# Neural Embeddings
## Word2vec(Doc2vec)
Word2vec is a neural network structure to generate word embedding by training the model on a supervised classification problem. In the word2vec architecture, the two algorithm names are “continuous bag of words” (CBOW) and “skip-gram” (SG). Word2vec only takes local contexts into account and does not take advantage of global context. 

Word2vec is a predictive model which learns their vectors in order to improve their predictive ability of Loss(target word | context words; Vectors), i.e. the loss of predicting the target words from the context words given the vector representations. In word2vec, this is cast as a feed-forward neural network and optimized as such using SGD, etc.

![model overview](word2vec.png)

1. Example: https://towardsdatascience.com/word2vec-from-scratch-with-numpy-8786ddd49e72

## GloVe
GloVe is a count-based model which learns their vectors by doing dimensionality reduction on the co-occurrence information. Firsly, construct a large matrix of (word * context) co-occurrence information. Secondly, factorize this matrix to a lower-dimensional (word * features) matrix, where each row yield a vector representation for each word. In the specific case of GloVe, the counts matrix is preprocessed by normalizing the counts and log-smoothing them.

1. The difference between GloVe and Word2vec: https://www.quora.com/How-is-GloVe-different-from-word2vec

# Transformer
## ELMo
![model overview](ELMo.gif)
- Unlike traditional word embeddings such as word2vec and GLoVe, the ELMo vector assigned to a token or word is actually a function of the entire sentence containing that word. Therefore, the same word can have different word vectors under different contexts.

1. Simple introduction: https://www.analyticsvidhya.com/blog/2019/03/learn-to-use-elmo-to-extract-features-from-text/

## BERT
Inportant: They employed masked language modeling. In other words, they hid 15% of the words and used their position information to infer them. Finally, they also mixed it up a little bit to make the learning process more effective.

InputExample Format(tsv): 
-  guid: Unique ID for the row
-  text_a: Text
-  text_b: A column of the same letter for all rows. BERT wants, but we don’t use
-  labels: The text for row (will be empty for test data)

InputFeature Requires:
-  Require: convert InputExample to InputFeature (purely numberical data)
-  Steps: 
    -  tokenizing embedding: tokenize the text, truncate the long sequence or pad the short sequence to the given sequence length (max 512, 128 ...)
    -  sentence embedding
    -  transformer positional embedding
    
InputFeature Format:
- input_ids: list of numberical ids for the tokenised text
- input_mask: will be set to 1 for real tokens and 0 for the padding tokens
- segment_ids: sigle sentence or multiple sentence
- label_ids: one-hot encoded labels for the text

Masked LM
- uncased BERT Base: 12 layers encoders, which embedding do you want, probably the sum of last 4 layer

Output
- "pooled_output" for classification tasks on an entire sentence
- "sequence_outputs" for token-level output

Estimate
- classification matrix

Next Sentence Prediction 

Difference

![model overview](BERT_ELMo.png)

1. https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270
2. https://medium.com/swlh/a-simple-guide-on-using-bert-for-text-classification-bbf041ac8d04
3. https://towardsdatascience.com/nlp-extract-contextualized-word-embeddings-from-bert-keras-tf-67ef29f60a7b
4. [movie setiment analysis](https://github.com/google-research/bert/blob/master/predicting_movie_reviews_with_bert_on_tf_hub.ipynb)

# Neural NLP Architectures
## MNL

## CNN
CNN works as a n-gram feature extractors for embeddings. 
- convolutional Layer(kernel/filter) is to extract the high-level features.
- pooling layer is to decrease the computational power required to process the data through dimensionality reduction. 
- fully connected layer with dropout and softmax output.

1. Simple explaination: https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53

## RNN(LSTMs and GRUs)


# Neural Networks

Regularization:
-  dropout: randomly turn off certain parts of our network while training
    -  decide that during this epoch or this mini-batch, a bunch of neurons will be turned off
    -  assign each neuron with a probability ps at random
    -  decide of the threshold: a value that will determine if the node is kept or not
    -  dropout is only used during training time, during test time, all the nodes of our network are always present
-  data augmentation: instead of feeding the model with the same pictures every time, we do small random transformations (a bit of rotation, zoom, translation, etc…) that doesn’t change what’s inside the image (for the human eye) but changes its pixel values. 
-  weight decay: multiply the sum of squares with another smaller number to the loss function
    -  Loss = MSE(y_hat, y) + wd * sum(w^2), wd is the weight decay
    -  w(t) = w(t-1) - lr * dLoss / dw => d(wd * w^2) / dw = 2 * wd * w (similar to dx^2/dx = 2x)
    -  generally a wd = 0.1 works pretty well
 
1. https://becominghuman.ai/this-thing-called-weight-decay-a7cd4bcfccab