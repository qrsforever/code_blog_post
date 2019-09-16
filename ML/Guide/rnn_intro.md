---

title: 循环神经网络RNN介绍

date: 2019-09-12 13:52:48
tags: [Guide, Graph]
categories: [ML]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/ML/Guide/rnn_intro.md)

<!-- vim-markdown-toc GFM -->

* [关键字](#关键字)
* [RNN(BPTT)](#rnnbptt)
    * [隐马尔可夫模型](#隐马尔可夫模型)
    * [BPTT](#bptt)
* [长期依赖](#长期依赖)
* [梯度消失](#梯度消失)
* [应用场景](#应用场景)
    * [语音识别](#语音识别)
    * [语言翻译](#语言翻译)
    * [股票预测](#股票预测)
    * [图像识别(图里的内容)](#图像识别图里的内容)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# 关键字

神经注意力模块(Attention) = 向前预测单元 + 后向回顾单元

# RNN(BPTT)

## 隐马尔可夫模型

Hidden Markov Model(HMM)

{% blockquote 高杨 <<白话深度学习与TensorFlow>> %}

马尔科夫链的核心是说, 在给定当前知识或信息的情况下, 观察对象的过去的历史状态, 对于将来的预测来说预测是无关
的. 也可以说, 在观察一个系统变化的时候, 它下一个状态(n+1)如何的概率只需观察和统计当前状态(n).

隐马尔科夫链是个双重随机过程, 不仅状态转移之间是个随机过程, 状态和输出之间也是个随机过程.

{% endblockquote %}

```{.graph .center caption="隐马尔可夫链" fileName="rnn_intro_g1"}
digraph G {
    graph [splines=ortho, rankdir=LR, nodesep=1, penwidth=0];
    edge  [style=solid];
    node  [shape=circle, label=""];

    subgraph cluster_0 {
        node  [shape=circle, style=dotted];

        x1 [label=<X<sub>1</sub>>];
        x2 [label=<X<sub>2</sub>>];
        x_ [label="......", penwidth=0];
        xt [label=<X<sub>T</sub>>];

        x1 -> x2 -> x_ -> xt [label="状态转移"];
    }

    subgraph cluster_1 {
        node  [shape=box];

        o1 [label=<O<sub>1</sub>>];
        o2 [label=<O<sub>2</sub>>];
        o_ [label="......", penwidth=0];
        ot [label=<O<sub>T</sub>>];
    }

    { rank=same; x1 -> o1; }
    { rank=same; x2 -> o2; }
    { rank=same; x_ -> o_ [penwidth=0, dir=none]; }
    { rank=same; xt -> ot; }
}
```

状态的改变是使用虚线表示$X_1$到$X_T$ (隐含状态链), 我们没法直接观察到, 而我们能够直接看到的是状态改变时带
来的观察值的变化$O_1$到$O_T$ (可见状态链). 可见状态之间没有直接的转换概率, 隐含状态和可见状态之间存在一个
概率叫做**输出概率**

训练模型:

通过输入$X_i$和$O_i$两个序列, 经过统计学模型训练, 最后得到两个矩阵, 一个是$X$之间的隐含状态转移关系的矩阵,
一个是$X$到$O$之间的输出概率矩阵.

## BPTT

Backpropagation Through Time:

The parameters are shared by all times in the rnn network, the gradient at each output depends not only
the current time steps but also the previous time steps. For example, in order to calculate the gradient
at $t=4$, we would need to backpropagate 3 steps and sum up the gradients.

![A recurrent neural network and the unfolding in time of the computation involved in its forward computation][bptt-1]

[bptt-1]: https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/rnn_classic_BPTT.png

Formulas:

$x_t$: one-hot vector, t时刻的输入. </br>
$s_t$: hidden state, t时候的隐藏状态, 通过前一个隐藏状态和当前输入计算出来的, $s_t = f(Ux_t + Ws_{t-1})$, f可以是tanh或relu. </br>
$o_t$: output, $o_t = softmax(Vs_t)$ </br>

模型里是蕴含着这样的逻辑的, 那就是前一次输入的向量$x_{t-1}$所产生的结果对于本次输出的结果是有一定的影响的,
甚至更远期的$x_{t-2}, x_{t-3} ··· ···$都"潜移默化"地在影响本次输出的结果.

There are a few things to note here:

{% blockquote wildml http://www.wildml.com/2015/09/recurrent-neural-networks-tutorial-part-1-introduction-to-rnns/ recurrent-neural-networks-tutorial-part-1-introduction-to-rnns %}

- You can think of the hidden state s_t as the memory of the network. s_t captures information about what
  happened in all the previous time steps. The output at step o_t is calculated solely based on the memory
  at time t. As briefly mentioned above, it’s a bit more complicated  in practice because s_t typically
  can’t capture information from too many time steps ago.

- Unlike a traditional deep neural network, which uses different parameters at each layer, a RNN shares
  the same parameters (U, V, W above) across all steps. This reflects the fact that we are performing the
  same task at each step, just with different inputs. This greatly reduces the total number of parameters
  we need to learn.

- The above diagram has outputs at each time step, but depending on the task this may not be necessary.
  For example, when predicting the sentiment of a sentence we may only care about the final output, not
  the sentiment after each word. Similarly, we may not need inputs at each time step. The main feature of
  an RNN is its hidden state, which captures some information about a sequence.

{% endblockquote %}

# 长期依赖

LSTM 解决避免长时期依赖(long-term dependency)的问题

# 梯度消失

LSTM 在某种程度上可以克服梯度消失问题.

![传统后向传播](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/rnn_grad_vanishing.gif)

![时间后向传播](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/rnn_time_grad_vanishing.gif)

图片来自[^1]

[^1]: https://baijiahao.baidu.com/s?id=1612358810937334377&wfr=spider&for=p

# 应用场景

## 语音识别

## 语言翻译

## 股票预测

## 图像识别(图里的内容)


# References

- [Understanding LSTM Networks](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)
- [译 Understanding LSTM Networks](https://www.cnblogs.com/mfryf/p/7904017.html)
- [如何深度理解RNN?——看图就好!](https://baijiahao.baidu.com/s?id=1612358810937334377&wfr=spider&for=pc)
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
- [recurrent-neural-networks-tutorial-part-1-introduction-to-rnns](http://www.wildml.com/2015/09/recurrent-neural-networks-tutorial-part-1-introduction-to-rnns/)
- [recurrent-neural-networks-tutorial-part-3-backpropagation-through-time-and-vanishing-gradients](http://www.wildml.com/2015/10/recurrent-neural-networks-tutorial-part-3-backpropagation-through-time-and-vanishing-gradients/)
- [deriving-back-propagation-on-simple-rnn-lstm](https://towardsdatascience.com/back-to-basics-deriving-back-propagation-on-simple-rnn-lstm-feat-aidan-gomez-c7f286ba973d)
