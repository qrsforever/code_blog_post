---

title: 梯度消失,梯度爆炸

date: 2019-08-27 13:29:56
tags: [ML]
categories: [Books]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/Books/ML/vanishing_exploding_gradients.md)

<!-- vim-markdown-toc GFM -->

* [梯度不稳定](#梯度不稳定)
    * [梯度消失](#梯度消失)
    * [梯度爆炸](#梯度爆炸)
* [实例](#实例)
* [解决办法](#解决办法)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# 梯度不稳定

前面层上的梯度是来自于后面层上梯度的乘乘积.当存在过多的层次时,就出现了内在本质上的不稳定场景,如梯度消失和梯度爆炸.

## 梯度消失

vanishing gradient

当前面层的梯度比后面层的梯度变化更小, 会引起vanishing gradient.

## 梯度爆炸

exploding gradient

当前面层的梯度比后面层的梯度变化更大, 会引起exploding gradient.

# 实例

1个输入层, 3个隐藏层, 1个输出层, 激活函数为$\sigma = sigmoid()$ 为例:

```{.graph .center caption="简单神经元网络" fileName="veg_g1"}
digraph G {
    graph [splines=ortho, rankdir=LR, ranksep=0.75, nodesep=0.5];
    edge  [style=solid, arrowhead=open, arrowtail=normal, fontsize=12];
    node  [label="", shape=circle, style=filled];
    {
        graph [style=invis];
        node  [color=grey];
        i_1 [label=<x<sub>0</sub>>];
    }
    subgraph cluster_hiden {
        graph [style=invis, label="hidden layer", style=dashed];
        node  [color=tan];
        h_1 [label=<b<sub>1</sub>>, fontsize=12];
        h_2 [label=<b<sub>2</sub>>, fontsize=12];
        h_3 [label=<b<sub>3</sub>>, fontsize=12];
    }
    {
        graph [style=invis];
        node  [color=grey];
        o_1 [label=<b<sub>4</sub>>, fontsize=12];
    }

    C [label="C", penwidth=0.0, style=bold]

    i_1 -> h_1 [label=<w<sub>1</sub>>];
    h_1 -> h_2 [label=<w<sub>2</sub>>];
    h_2 -> h_3 [label=<w<sub>3</sub>>];
    h_3 -> o_1 [label=<w<sub>4</sub>>];
    o_1 -> C;
}
```

$$
\begin{align*}
a_1 &= \sigma(z_1 = w_1 x_0 + b_1) \\
a_2 &= \sigma(z_2 = w_2 a_1 + b_2) \\
a_3 &= \sigma(z_3 = w_3 a_2 + b_3) \\
a_4 &= \sigma(z_4 = w_4 a_3 + b_4) \\
C &= Cost(a_4, y)
\end{align*}
$$

给出sigmoid函数以及导函数图:

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/Books/ML/sigmoid.png)

然后计算梯度:

$$
\begin{align*}
\dfrac{\partial{C}}{\partial{b_1}} &= \sigma(z_1)' w_2 \sigma(z_2)' w_2 \sigma(z_3)' w_3 \sigma(z_4)' w_4 \dfrac{\partial{C}}{\partial{a_4}} \\
\dfrac{\partial{C}}{\partial{b_3}} &= \sigma(z_3)' w_3 \sigma(z_4)' w_4 \dfrac{\partial{C}}{\partial{a_4}}
\end{align*}
$$

从sigmoid的导函数看大于4或小于-4时, 值变得接近于0, 而且导函数的最大值为0.25, 如果权重初始化时值比较小, 上
面的计算公式可以看出, 最后计算出的梯度, 是非常小的, 且越靠前面的层的梯度越小(一直连乘一个小于1的数), 所以
会出现梯度消失的情况.
同样道理, 如果权重值初始化值很大(或者过程中计算出的值大于1), 经过连乘之后会发现前面层的值越来越大, 引起梯
度爆炸.

# 解决办法

使用ReLU,maxout等替代sigmoid.

# References

- <https://www.cnblogs.com/DjangoBlog/p/7699664.html>
