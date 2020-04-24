---

title: 梯度消失,梯度爆炸

date: 2019-08-27 13:29:56
tags: [Graph, Guide]
categories: [ML]

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


## clip gradients

- 首先设置一个梯度阈值：clip_gradient
- 在后向传播中求出各参数的梯度，这里我们不直接使用梯度进去参数更新，我们求这些梯度的l2范数
- 然后比较梯度的l2范数||g||与clip_gradient的大小
- 如果前者大，求缩放因子clip_gradient/||g||,　由缩放因子可以看出梯度越大，则缩放因子越小，这样便很好地控制了梯度的范围
- 最后将梯度乘上缩放因子便得到最后所需的梯度


draft:

```
常见的 gradient clipping 有两种做法

根据参数的 gradient 的值直接进行裁剪
根据若干参数的 gradient 组成的 vector 的 L2 norm 进行裁剪
第一种做法很容易理解，就是先设定一个 gradient 的范围如 (-1, 1), 小于 -1 的 gradient 设为 -1， 大于这个 1 的 gradient 设为 1.

第二种方法则更为常见，先设定一个 clip_norm, 然后在某一次反向传播后，通过各个参数的 gradient 构成一个
vector，计算这个 vector 的 L2 norm（平方和后开根号）记为 LNorm，然后比较 LNorm 和 clip_norm 的值，若 LNorm
<= clip_norm 不做处理，否则计算缩放因子 scale_factor = clip_norm/LNorm ，然后令原来的梯度乘上这个缩放因子
。这样做是为了让 gradient vector 的 L2 norm 小于预设的 clip_norm。
```

# References

- <https://www.cnblogs.com/DjangoBlog/p/7699664.html>

- <https://blog.csdn.net/u010814042/article/details/76154391>

- <https://machinelearningmastery.com/how-to-avoid-exploding-gradients-in-neural-networks-with-gradient-clipping>

- <https://blog.floydhub.com/gru-with-pytorch/>
