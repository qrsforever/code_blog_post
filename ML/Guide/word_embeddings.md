---

title: (draft)词向量发展过程

date: 2019-11-26 10:55:04
tags: [Draft, NLP]
categories: [ML]

---

<!-- more -->

# Draft

Text representation: 如何让计算机明白单词的含义(understand the concepts of words)?

word vectors: words or phrases from a given language vocabulary are mapped to vectors of real numbers.


# Traditional vector representation

Bag of Words (aka BoW)

don’t encode any information with regards to the meaning of a given word.

共现矩阵

SVD（奇异值分解）

# Neural Embeddings

## Word2Vec

Continuous bag-of-words (CBOW)

Continuous skip-gram

GloVe

FastText



# References

1. [从Word Embedding到Bert模型—自然语言处理中的预训练技术发展史][1]
2. [Word Embeddings: An Introduction to the NLP Landscape][2]
3. [词向量发展史-共现矩阵-SVD-NNLM-Word2Vec-Glove-ELMo][3]
4. [Word Vectors and NLP Modeling from BoW to BERT][4]


[1]: https://zhuanlan.zhihu.com/p/49271699
[2]: https://medium.com/analytics-vidhya/word-embeddings-an-introduction-to-the-landscape-dcf20cf391a1
[3]: https://blog.csdn.net/m0_37565948/article/details/84989565
[4]: https://towardsdatascience.com/beyond-word-embeddings-part-2-word-vectors-nlp-modeling-from-bow-to-bert-4ebd4711d0ec
