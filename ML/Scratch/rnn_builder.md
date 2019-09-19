---

title: (draft)RNN代码实例(numpy)

date: 2019-09-18 19:51:24
tags: [Scratch, Python]
categories: [ML]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/ML/Scratch/rnn_builder.md)

<!-- vim-markdown-toc GFM -->

* [Formulas](#formulas)
* [Notebook](#notebook)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Formulas

## 符号

 符号 | 解释
:--- | :---
$K$ | 词汇表的大小  
$T$ | 句子的长度  
$H$ | 隐藏层单元数 
$N$ | 训练数 
$x \in \mathbb{R}^{T \times 8000}$ | 输入
$o \in \mathbb{R}^{T \times 8000}$ | 输出
$s \in \mathbb{R}^{T \times 100}$ | 隐藏状态, 没算上$s_{-1}$
$U \in \mathbb{R}^{100 \times 8000}$ | 输入权重
$V \in \mathbb{R}^{8000 \times 100}$ | 输出权重
$W \in \mathbb{R}^{100 \times 100}$ | 隐藏状态转移权重

## 公式

$$
\begin{aligned}
d_3 & \triangleq \big(\hat{y}_3 - y_3 \big) \cdot V \cdot \big(1 - s_3 ^ 2 \big) \\
d_2 & \triangleq d_3 \cdot W \cdot \big(1 - s_2 ^ 2 \big) \\
d_1 & \triangleq d_2 \cdot W \cdot \big(1 - s_1 ^ 2 \big) \\
d_0 & \triangleq d_1 \cdot W \cdot \big(1 - s_0 ^ 2 \big) \\ \\
\frac{\partial{E_3}}{\partial{V}} &= \frac{\partial{E_3}}{\partial{\hat{y}_3}} \frac{\partial{\hat{y}_3}}{\partial{z_3}} \frac{\partial{z_3}}{\partial{V}} \\
&= (\hat{y}_{3} - y_3)  s_3 \\ \\
\frac{\partial{E_3}}{\partial{W}} &= \frac{\partial{E_3}}{\partial{\hat{y}_3}} \frac{\partial{\hat{y}_3}}{\partial{z_3}} \frac{\partial{z_3}}{\partial{s_3}} \frac{\partial{s_3}}{\partial{W}}  \\
& \triangleq d_3 s_2 + d_2 s_1 + d_1 s_0 + d_0 \cdot s_{-1}  \\ \\
\frac{\partial{E_3}}{\partial{U}} &= \frac{\partial{E_3}}{\partial{\hat{y}_3}} \frac{\partial{\hat{y}_3}}{\partial{z_3}} \frac{\partial{z_3}}{\partial{s_3}} \frac{\partial{s_3}}{\partial{U}}  \\
& \triangleq d_3 x_3 + d_2 x_2 + d_1 x_1 + d_0 \cdot x_0  \\
\end{aligned}
$$

# Notebook

{% asset_jupyter python3 notebook/rnn_builder.ipynb 300 %}

# References

- <https://songhuiming.github.io/pages/2017/08/20/build-recurrent-neural-network-from-scratch/>
