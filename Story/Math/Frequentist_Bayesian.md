---

title: Frequentist和Bayesian之前的故事
date: 2018-10-30 10:51:02
tags: [ Math ]
categories: [ Story ]

---

[原文](http://blog.sina.com.cn/s/blog_a8fead9b01014kj7.html)

在统计学中，很多工程师会惊讶于在统计学居然存在两个本质上截然对立的学派，他们是如此的对立以至于他们对于概率的定义都是截然不同的，这
就是Frequentist和Bayesian统计学派之间的故事。

对于Frequentist来说，概率是一个实验重复很多次后的期望频率，而Bayesian观点则认为概率从本质上来说是我们对于一个事件的信心（Degree of
Belief），它用来描述我们认为一个事件有多大可行性发生。

所以对于Frequentist来说，均值是一个确定且存在的数，只是我们不知道也没有办法知道他的确切数值，当一个Frequentist对数据进行分析的时候
，他会建立一个Confidence Interval，其中心在所有数据的均值处。

这里就有一个很有意思的事情，因为对于Frequentist来说均值确实存在而且确定，所以它或者在区间内或者不在区间内，所以Frequentist不能说实
际均值有95%的概率处在这个Confidence Interval中，因为实际均值是一个固定的数，并不是随机的，所以他们只能采用一个迂回的说法：对于95%
的随机样本，这样算法产生的Confidence Interval包含了实际均值。

而对于Bayesians来说，世界确实截然不同的，他们认为只有数据才是真实的，实际均值也是一个随机的变量，所以他们可以说实际均值更有可能是
某个值而不是另外某个值（基于数据和先验知识），于是Bayesians构建一个Credible Interval，他们说实际均值有95%的概率在Credible Interval
中，这是Frequentist所不能接受的一种说法。

在1990年以前，很少有人从事Bayesian Analysis，原因是Bayesian Analysis需要大量的计算，而当时的计算机能力很弱，最近一段时间Bayesian
Method发展很迅速，Sampling Method和Variational Method的出现使得Bayesian方法逐渐变得Popular。
