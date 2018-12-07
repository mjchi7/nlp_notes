# What the hell is `tf.contrib.seq2seq.TrainingHelper` anyway?

Surprisingly, this function has been rarely mentioned; perhaps it is a very straightforward and I have missed some points here and there, but I have struggle a lot with what this function is supposed to do. 

### Decoding sequence
To understand it, first we need to know the decoding sequence during **training**. During training, at $t=1$, $x_{t=1}$ will most probably be some token to indicate a start of sentence (such as \<GO>, \<S>, and etc.) and $h_{t=0}$ is the hidden state vector from encoder. Passing all these variables into the decoder's RNN cell, we should get an output $y_{t=1}$. As we are in training, we do not want to feed this $y_{t=1}$ back into RNN cell as $x_{t=2}$ (which is the common thing to do during inference). Instead, we have a set of "label" data that tells the decoder what should be fed into the RNN cell as $x_{t=2}$. Therefore we need this `TrainingHelper()` class. 

### `TrainingHelper()`
It takes in two parameters:

1. `embedding_vectors`
This embedding_vectors are obtained by using embedding lookup in the following manners:
	```python
	embedding_vectors = tf.nn.embedding_lookup(word_embedding, label_data)
	```
	Where the label_data will be the ids of target sentence. For instance: [2,3,10,6].

2. `sequence_length`
This parameter tells the decoder for this batch element, how many step it should run the RNN cell for. (recall explanation for `tf.nn.dynamic_rnn`, where computation graph are dynamic in the sense that anything beyond `sequence_length` will be ignored.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMjgyNjk4M119
-->