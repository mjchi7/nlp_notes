# Paper Digest: Advances in Pre-Training Distributed Word Representations 

## Quick facts
1. Based on Continuous Bag of Words (cbow) of *word2vec* model.
2. Attempts to improve word embedding models by integrating several independent techniques.
3. Outperforms GloVe model in several tests by significant margins.

## Highlights
The authors of this paper has modified the original cbow model using several techniques derived by others in different papers (noted below). All the techniques are derived in order to better learn word representation but they have never been made to work together (until this paper comes along). The techniques mentioned are:

1. Position Dependent Features (by [Mnih and Kavukcuoglu (2013)](https://papers.nips.cc/paper/5165-learning-word-embeddings-efficiently-with-noise-contrastive-estimation.pdf)
2. Phrase Representation (by [Mikolov et al. (2013)](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf)
3. Subword information (by [Bojanowski et al. 2017](https://arxiv.org/pdf/1607.04606.pdf))

By integrating all of these techniques together into cbow model, the word embedding learned scores much higher in the several word embedding test.

Author of this paper has also reports the finding that **de-duplicating sentences** in large corpora (such as Common Crawl) is **an important pre-processing step** which can significantly improves the quality of word embedding learned.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NzMyMDE5NDBdfQ==
-->