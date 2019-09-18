---

title: 丢弃法

date: 2019-08-27 13:28:58
tags: [Guide]
categories: [ML]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/Books/ML/dropout.md)

<!-- vim-markdown-toc GFM -->

* [What](#what)
* [DrawIt](#drawit)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# What

Co-Adaptation:

{% blockquote "Chitta Ranjan" https://towardsdatascience.com/simplified-math-behind-dropout-in-deep-learning-6d50f3f47275 "Understanding Dropout with the Simplified Math behind it" %}

In such a network, if all the weights are learned together it is common that some of the connections will
have more predictive capability than the others.  In such a scenario, as the network is trained
iteratively these powerful connections are learned more while the weaker ones are ignored. Over many
iterations, only a fraction of the node connections is trained. And the rest stop participating.  This
phenomenon is called co-adaptation.

{% endblockquote %}

Ensemble Average:

{% blockquote stackexchange https://math.stackexchange.com/questions/1339012/what-does-ensemble-average-mean "what does ensemble average mean?" %}

Ensemble average is properties of **stochastic processes**, which are like random variables but
take the form of waveforms. Ensemble average is analogous to **expected** value or mean, in that it represents
a sort of "average" for the stochastic process. It is a function of the same variable as the stochastic
process, and when evaluated at a particular value denotes the average value that the waveforms will have
at that same value.

{% endblockquote %}


Dropout:

{% blockquote Hinton https://arxiv.org/pdf/1207.0580.pdf "Improving neural networks by preventing co-adaptation of feature detectors" %}

When a large feedforward neural network is trained on a small training set, it typically performs poorly
on held-out test data. This "overfitting" is greatly reduced by randomly omitting half of the feature
detectors on each training case. This prevents complex co-adaptations in which a feature detector is only
helpful in the context of several other specific feature detectors. Instead, each neuron learns to detect
a feature that is generally helpful for producing the correct answer given the combinatorially large
variety of internal contexts in which it must operate. Random "dropout" gives big improvements on many
benchmark tasks and sets new records for speech and object recognition.

{% endblockquote %}

当一个复杂的前馈神经网络被训练在小的数据集时,容易造成过拟合.为了防止过拟合,可以通过阻止特征检测器的共同作
用来提高神经网络的性能.

大规模的神经网络中的两个难题:(1)过拟合; (2)费时

解决第一个**过拟合**问题可以采用模型组合(ensemble)方式, 这样就会带来第二个**费时**问题, 在这种背景下
**Dropout**被提出来了.

> Dropout强迫一个神经单元,和随机挑选出来的其他神经单元共同工作,达到好的效果.消除减弱了神经元节点间的联合
适应性,增强了泛化能力.

# DrawIt

Network with Bernoulli gate:

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/Books/ML/dropout_network.png){ .center }

Dropout train with probability 0.5:

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/Books/ML/dropout_train.jpg){ .center }

Dropout training in a simple network. For each training example, feature detector units are dropped with
probability 0.5. The weights are trained by backpropagation (BP) and shared with all the other examples.

Dropout test:

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/Books/ML/dropout_test.jpg){ .center }

Dropout prediction in a simple network. At prediction time, all the weights from the feature detectors to
the output units are halved.

未完!!!

# References

#. <https://towardsdatascience.com/simplified-math-behind-dropout-in-deep-learning-6d50f3f47275>
#. <https://www.sciencedirect.com/science/article/pii/S0004370214000216>
#. <https://blog.csdn.net/stdcoutzyx/article/details/49022443>
#. <https://www.cnblogs.com/bonelee/p/8127451.html>
#. <https://papers.nips.cc/paper/4878-understanding-dropout.pdf>
#. <http://www.jmlr.org/papers/volume15/srivastava14a/srivastava14a.pdf>
