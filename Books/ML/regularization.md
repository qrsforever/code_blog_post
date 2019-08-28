---

title: 正则化

date: 2019-08-27 10:24:46
tags: [ML]
categories: [Books]

---

<!-- vim-markdown-toc GFM -->

* [What](#what)
* [Cost](#cost)
    * [L1正则化项](#l1正则化项)
    * [L2正则化项](#l2正则化项)
    * [可视化](#可视化)

<!-- vim-markdown-toc -->

<!-- more -->

# What

{% blockquote "高扬" https://github.com/azheng333/DeepLearningAndTensorFlow "白话深度学习与TensorFlow" %}
模型越简洁(抽象)泛化性越好<br>
正则化过程就是帮助我们找到更为简洁的描述方式的量化过程.<br>
正则化项的加入会帮助模型找到描述更为简洁的方式从而提高模型的泛化能力, 一定程度上避免过拟合.
{% endblockquote %}

# Cost

损失函数的改造 $C = C_0 + R$

经验损失($C_0$): 模型拟合结果与标签之间的参差总和, 结果越大, 越**欠拟合**

结构损失($R$惩罚因子): 保证模型的泛化性良好, 防止**过拟合**.

**拟合程度好不代表泛化性能就一定好**

## L1正则化项

$$
\begin{align*}
C &= C_0 + \dfrac{\lambda}{n}\sum_{\omega}|\omega| \\
\dfrac{\partial{C}}{\partial{\omega}} &= \dfrac{\partial{C_0}}{\partial{\omega}} + \dfrac{\lambda}{n}sign(\omega) \\
\omega' &= \omega - \eta \dfrac{\partial{C_0}}{\partial{\omega}} - \eta \dfrac{\lambda}{n}sign(\omega)
\end{align*}
$$

其中$\lambda$是正则化系数或惩罚系数,表示对这个部分(结构损失)有多"重视", 如果我们很重视结构风险,或者说很不
希望结构风险太大,那我们就加大$\lamdba$,迫使整个损失函数向着权值$\omega$减小的方向快速移动 .

## L2正则化项

$$
\begin{align*}
C &= C_0 + \dfrac{\lambda}{2n}\sum_{\omega}\omega^2 \\
\dfrac{\partial{C}}{\partial{\omega}} &= \dfrac{\partial{C_0}}{\partial{\omega}} + \dfrac{\lambda}{n}\omega \\
\omega' &= \omega - \eta \dfrac{\partial{C_0}}{\partial{\omega}} - \eta \dfrac{\lambda}{n}\omega
\end{align*}
$$

## 可视化

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/Books/ML/regularization_l1_l2.png){ .center }

{% blockquote "高扬" https://github.com/azheng333/DeepLearningAndTensorFlow "白话深度学习与TensorFlow" %}
假设在一个模型中只有两个维度$\omega_1$和$\omega_2$作为待定系数,最终的理想解在圆心的位置,当然这里画出来的是
在第一象限,但是实际上它也会出现在别的位置.由于初始化的时候$\omega_1$和$\omega_2$可能会在别的位置,当然也会
在二 三 四象限中.在训练的过程中会逐步从这个初始化的位置向圆心靠拢.<br>
圆心周围的一圈一圈的线其实是损失
函数等高线,也就是说当$\omega_1$和$\omega_2$所组成的坐标点$(\omega_1,\omega_2)$在这一圈上的任意位置都会产
生同样大小的损失函数,而由于初始化位置不确定,所以可能会出现在一圈上的任意位置,那么显然远离坐标系圆点(0,0)的
$(\omega_1,\omega_2)$点会产生更大 的结构风险,因为其拥有更大的$\omega_1$和$\omega_2$值,更为不简洁.
{% endblockquote %}

## Dropout

pass
