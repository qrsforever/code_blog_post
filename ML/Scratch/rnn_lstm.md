---

title: (draft)LSTM算法numpy实现

date: 2019-09-19 17:11:22
tags: [Scratch, Python]
categories: [ML]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/ML/Scratch/rnn_lstm.md)


<!-- vim-markdown-toc GFM -->

* [W,U合并](#wu合并)
* [Logits](#logits)
* [Codes](#codes)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# W,U合并

在进入代码环节之前, 先介绍一下代码中出现的W,U参数的合并, 如下图

```
              |<---H---->|<---------K---------->|
           ---+---------------------------------+
           ^  |          |                      |
           |  |          |                      |
           H  |    W     |          U           |
           |  |          |                      |
           v  |          |                      |
           ---+---------------------------------+
                 Wh_{t-1}          Ux_t
```

前: $s_t = \phi(Wh_{t-1} + Ux_t + b)$

后: $s_t = \phi(W' <h_{t-1}, x_t> + b) = \phi(W' z_t + b) $

从空间维度分析:

$W \in \mathbf R^{H \times H}$, $U \in \mathbf R^{H \times K}$, $W' \in \mathbf R^{H \times (H+K)}$

$h_{t-1} \in \mathbf R^{H \times 1}$, $x_t \in \mathbf R^{K \times 1}$, $z_t \in \mathbf R^{(H+K) \times 1}$

# Logits

在代码中出现, 把这个单词拆开log + it(它odds) + s(复数), 即对它们(odds)取log. odds是"几率, 胜率", 一个例子
搞定:

assume: $\text{ if } P = \dfrac {1}{5}, 1-P = \dfrac{4}{5} \text { then } Odds(A) = log \dfrac{1/5}{4/5}$

logits (未归一化的概率)一般作为softmax的输入参数(归一化).

# Codes

[百度云盘Datasets](https://pan.baidu.com/s/1gAFZ9gSf4pHJBt5W6_PgPQ "提取码: gxk4")

{% asset_jupyter python3 notebook/rnn_lstm.ipynb 1200 %}

# References

- <https://blog.varunajayasiri.com/numpy_lstm.html>
- <https://github.com/revsic/numpy-rnn>
