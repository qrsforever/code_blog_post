---

title: (draft)Pytorch快速入门

date: 2019-08-25 17:42:56
tags: [Pytorch, Draft]
categories: [ML]

---


<!-- vim-markdown-toc GFM -->

* [入门教程](#入门教程)
    * [重点](#重点)

<!-- vim-markdown-toc -->

# 入门教程

学习Pytorch必先看此书[GITHUB:zergtant/pytorch-handbook][1], 然后看[官网文档][2]

## 重点

**为什么激活函数都是非线性的?**

{% blockquote zergtant, https://github.com/zergtant https://github.com/zergtant/pytorch-handbook/blob/master/chapter2/2.3-deep-learning-neural-network-introduction.ipynb "神经网络简介" %}

在神经网络的计算过程中，每层都相当于矩阵相乘，无论神经网络有多少层输出都是输入的线性组合，就算我们有几千层
的计算，无非还是个矩阵相乘，和一层矩阵相乘所获得的信息差距不大，所以需要激活函数来引入非线性因素，使得神经
网络可以任意逼近任何非线性函数，这样神经网络就可以应用到众多的非线性模型中，增加了神经网络模型泛化的特性。

{% endblockquote %}

[1]: https://github.com/zergtant/pytorch-handbook
[2]: https://pytorch.org/tutorials/
