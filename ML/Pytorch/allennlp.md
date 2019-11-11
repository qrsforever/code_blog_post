---

title: (draft)AllenNLP框架

date: 2019-10-23 13:56:37
tags: [Pytorch, Draft]
categories: [ML]

---



AllenNLP是一个相对成熟的基于深度学习的NLP工具包,它 构建于 PyTorch 之上,它的设计遵循以下原则:
(1)超模块化和轻量化.你可以使用自己喜欢的组件与 PyTorch 无缝连接.
(2)经过广泛测试,易于扩展.测试覆盖率超过 90%,示例模型为你提供了很好的模板.
(3)真正的填充和覆盖,让你可以毫无痛苦地轻松实现正确的模型.

(4)易于实验.可以通过符合 json 规范的全面记录重现实验过程.


-----------------------------------------------------------------

包含的几个大类如下:

Machine Comprehension:机器阅读

Semantic Role Labeling:语义标注

Coreference Resolution:指代消解

Textual Entailment:文本蕴涵

Named Entity Recognition:命名实体识别

Constituency Parsing:成分句法分析


语言推理任务 

问答和常识推理任务

语义相似度和分类任务


Sentiment Analysis [情感分析](http://www.realworldnlpbook.com/blog/training-sentiment-analyzer-using-allennlp.html)

-----------------------------------------------------------------

Word Representations 词表示


传统的词向量（例如word2vec）是上下文无关的

1. CEO of apple

2. eat an apple

举个例子：针对某一词多义的词汇 w="苹果"
文本序列1=“我 买了 六斤 苹果。”
文本序列2=“我 买了一个 苹果 7。”
上面两个文本序列中都出现了“苹果”这个词汇，但是在不同的句子中，它们的含义显示是不同的，一个属于水果领域，一个属于电子产品呢领域，如果针对“苹果”这个词汇同时训练两个词向量来分别刻画不同领域的信息呢？答案就是使用ELMo。

在EMLo中，他们使用的是一个双向的LSTM语言模型，由一个前向和一个后向语言模型构成，目标函数就是取这两个方向语言模型的最大似然。


-----------------------------------------------------------------

Open AI GPT : Generative Pre-Training


-----------------------------------------------------------------

BERT: Biderectional Encoder Representations from Transformers

-----------------------------------------------------------------

深度学习中Embedding层

[推荐阅读](https://spaces.ac.cn/archives/4122)

词向量，英文名叫Word Embedding，按照字面意思，应该是词嵌入。

词向量跟Word2Vec(google 先入为主, 把word to vector)不能在等同了, 后来有词ID映射为向量等

[有什么用](https://blog.csdn.net/u010412858/article/details/77848878)

嵌入层embedding在这里做的就是把单词“deep”用向量[.32, .02, .48, .21, .56, .15]来表达。然而并不是每一个单词
都会被一个向量来代替，而是被替换为用于查找嵌入矩阵中向量的索引。

<https://blog.csdn.net/jiangpeng59/article/details/77533309>

[Input层与embedding层](https://blog.csdn.net/yyhhlancelot/article/details/86534793)

embedding，比如word2vec啊，glove啊，fasttext啊。它们都是经过不同的语料以及不同的训练方法训练而成的

[Blog List](https://blog.csdn.net/jiangjiang_jian/article/details/81194582)

单词嵌入是使用密集的矢量表示来表示单词和文档的一类方法


定义一个词汇表为200的嵌入层（例如从0到199的整数编码的字，包括0到199），一个32维的向量空间，其中将嵌入单词
，以及输入文档，每个单词有50个单词。
`Embedding(input_dim=200, output_dim=32, input_length=50)`

独热编码one-hot

-----------------------------------------------------------------

Glove词向量 Global Vectors for Word Representation

https://nlp.stanford.edu/projects/glove/

-----------------------------------------------------------------


词性标注器:

A Part-Of-Speech Tagger (POS Tagger) is a piece of software that reads text in some language and assigns
parts of speech to each word (and other token), such as noun, verb, adjective, etc.

[Part-of-Speech 标记 含义](https://blog.csdn.net/lyjxcz/article/details/17082341)


POS词性标注

NER实体识别

-----------------------------------------------------------------

Spacy功能简介 可以用于进行分词,命名实体识别,词性识别等等


-----------------------------------------------------------------

图解BiDAF中的单词嵌入、字符嵌入和上下文嵌入

<https://www.jiqizhixin.com/articles/2019-10-09>

<https://www.jianshu.com/p/3af100e52568>

整个机器理解模型是一个层次化多阶段的过程， 包括以下六个层次：

    字符嵌入层（Character embedding layer）
    其主要作用是将词映射到一个固定大小的向量， 这里我们使用字符水平的卷积神经网络（Character level CNN）， 该网络由Kim在2014年提出， 我们后续会对部分细节进行补充。
    词嵌入层（Word embedding layer）
    这里是使用预先训练的词嵌入模型， 将每一个词映射到固定大小的向量。
    上下文嵌入层（Contextual embedding layer）
    主要作用是给每一个词加一个上下文的线索（cue）， 前三层都是对问题和上下文进行应用
    注意流层（Attention flow layer）
    这里是组合问题和上下文的向量， 生成一个问题-察觉的特征向量集合。
    模型层（Modeling layer）
    本文是使用循环神经网络（RNN）来对上下文进行扫描
    输出层（Output layer）
    该层提供对问题的回答

-----------------------------------------------------------------

词向量: 传统方式(共现矩阵) 和 语言模型

https://blog.csdn.net/ibelieve8013/article/details/88323271

[数据集](http://www.sohu.com/a/270730128_100191017)

[词向量发展史](https://blog.csdn.net/m0_37565948/article/details/84989565)

-----------------------------------------------------------------

[NLP 领域最优秀的 8 个预训练模型](https://www.infoq.cn/article/1fu*vYWCD8PlIartPZYV)
[NLP 领域最优秀的 8 个预训练模型](https://www.analyticsvidhya.com/blog/2019/03/pretrained-models-get-started-nlp/)

迁移学习本质上是在一个数据集上训练模型，然后对该模型进行调整，以在不同的数据集上执行不同的自然语言处理功能
。

自然语言处理应用能够快速增长，很大程度上要归功于通过预训练模型实现迁移学习的概念。在自然语言处理的背景下，
迁移学习本质上是在一个数据集上训练模型，然后对该模型进行调整，以在不同的数据集上执行不同的自然语言处理功能
。

这一突破，使得每个人都能够轻松地完成任务，尤其是那些没有时间、也没有资源从头开始构建自然语言处理模型的人们
。对于想要学习或过渡到自然语言处理的初学者来讲，它也堪称完美。

为什么要使用预训练模型？作者已尽其所能设计了基准模型。我们可以在自己的自然语言处理数据集上使用预训练模型，
而不是从头构建模型来解决类似的自然语言处理问题。

尽管仍然需要进行一些微调，但它已经为我们节省了大量的时间和计算资源。


-----------------------------------------------------------------

Tokenizer是把你的文本转换成单词

TokenIndexer是给这些形式编个号, 把最终的文本转换成序号表示的形式
TokenIndexer可以自动的为你生成单词编号，字母编号，pos_tags的编号。


`token_embedders`用于将index后的词转为tensor。常用的是Embedding类（可以读取预训练词向量）和TokenCharactersEncoder类。

对于TextField，需要用TextFieldEmbedder，更具体地，BasicTextFieldEmbedder。
BasicTextFieldEmbedder用来管理多个token_embedder，这样单词可以有多种嵌入方式，嵌入之后进行拼接。

-----------------------------------------------------------------

- 分词，帮你用spacy，NLTK，或者简单的按空格分词处理。

- 数据集的读取，它内置了很多数据集的读取，你可以在通过学习它的读取方式，在它的基础上对自己需要的数据集进行读取。

- 转化词向量过程: Glove，ELMo，BERT等常用的都可以直接使用，需要word，char粒度的都可以。


<https://www.cnblogs.com/huangyc/p/9861453.html>

语言模型分为统计语言模型和神经网络语言模型

-----------------------------------------------------------------

Seq2Seq 序列问题和其他的机器学习问题最显著的一个区别就是序列中的值相互之间是有一个顺序的

seq2seq学习的核心是使用循环神经网络将可变长度的输入序列映射到可变长度的输出序列

## References

#. <https://github.com/allenai/allennlp>

#. [realworldnlpbook](https://github.com/mhagiwara/realworldnlp)

#. <https://www.cnblogs.com/robert-dlut/p/9824346.html>

#. <https://blog.csdn.net/m0_38133212/article/category/8640328/>

#. [allennlp使用教程-简单](https://www.jianshu.com/p/17abfefc1b5b)
