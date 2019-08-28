---

title: 归一化

date: 2019-08-27 09:58:25
tags: [ML]
categories: [Books]

---


<!-- vim-markdown-toc GFM -->

* [Why](#why)
* [Method](#method)
    * [0-1标准化](#0-1标准化)
    * [z-score标准化](#z-score标准化)
    * [sigmoid函数](#sigmoid函数)
    * [References](#references)
* [TODO](#todo)

<!-- vim-markdown-toc -->

<!-- more -->

# Why

为什么需要归一化

{% blockquote "高扬" https://github.com/azheng333/DeepLearningAndTensorFlow "白话深度学习与TensorFlow" %}

  在机器学习的过程中, 一个由于计数单位的影响导致分布范围较宽广的值和 一个分布范围较窄小的值会在训练过程中有
着不同的影响能力,结果主要是会引起模型对某些值过于敏感或者不敏感,而这种情况其实是我 们不愿看到的一种天然由
外界强加给系统的“不公平”的情况。克服的办法也是有的,那就是使用归一化这样一个操作过程:**把数据的大小分布
压缩或框定在一个比例协调的范围之内**。

{% endblockquote %}

国家收入: 人民币, 韩币, 美元

# Method

## 0-1标准化

max-min scale

$$
x_{normalization} = \dfrac{x - x_{min}}{x_{max} - x_{min}}
$$

转化后: 0 <= x <= 1

## z-score标准化

z-score scale

$$
x_{normalization} = \dfrac{x - \mu}{\sigma}
$$

转化后: x ~ (0, 1)

## sigmoid函数

$$
x_{normalization} = \dfrac{1}{1 + e^{-x}}
$$

## References

#. <https://www.cnblogs.com/yhll/p/9857274.html>

转化后: 0 < x < 1, 集中在0.5附近

# TODO

**每层网络都可以进行归一化**
