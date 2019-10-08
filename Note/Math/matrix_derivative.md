---

title: (drfat)矩阵求导

date: 2019-09-17 16:04:21
tags: [Math]
categories: [Note]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/Note/Math/matrix_derivative.md)

<!-- vim-markdown-toc GFM -->

* [注解](#注解)
* [一元微分: 导数](#一元微分-导数)
* [多元微分: 梯度](#多元微分-梯度)
* [矩阵导数](#矩阵导数)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# 注解

符号 | 表示
---- | ----
$x$ | 标量
$\mathbf x$ | 粗体小写, 列向量
$X$ | 大些字母, 矩阵


# 一元微分: 导数

$$
df = f'(x)dx;
$$

# 多元微分: 梯度

$$
\begin{align*}
df &= \sum_{i=1}^n \dfrac{\partial{f}}{\partial{x_i}} dx_i \\
   &= {\dfrac{\partial{f}}{\partial{\mathbf x}}}^T d\mathbf x
\end{align*}
$$

第一个等号是全微分形式;

第二个等号是梯度与微分的关系: $\dfrac{\partial{f}}{\partial{\mathbf x}}$ (n x 1)梯度向量与微分向量$d\mathbf x$ (n x 1)的内积.

# 矩阵导数

$$
\begin{align*}
df &= \sum_{i=1}^{m}\sum_{j=1}^{n} \dfrac{\partial f}{\partial X_{ij}} dX_{ij} \\
   &= tr \bigg({\dfrac{\partial f}{\partial X}}^T dX \bigg)
\end{align*}
$$

`tr`表示方阵的迹$tr(AB)=\sum_{i,j}A_{i,j}B{ij}$, 对角线元素的和, tr的性质:

1. 标量的迹: $a = tr(a)$
2. 转置: $tr(A^T) = tr(A)$
3. 线性: $tr(A \pm B) = tr(A) \pm tr(B)$
4. 乘法交换: $tr(AB) = tr(BA)$
5. 逐元素乘法交换: $tr(A^T(B \bigodot C)) = tr((A \bigodot B)^TC)$

矩阵微分运算法则:

1. 加减法: $d(X \pm Y) = dX \pm dY$
2. 乘法: $d(XY) = (dX)Y + XdY$
3. 转置: $d(X^T) = (dX)^T$
4. 迹: $dtr(X) = tr(dX)$
5. 逆: $dX^{-1} = -X^{-1}dX X^{-1}$
6. 逐元素乘法: $d(X \bigodot Y) = dX \bigodot Y + X \bigodot dY$
7. 逐元素函数: $d\sigma(X) = \sigma'(X) \bigodot dX$

# References

- [矩阵求导术1](https://zhuanlan.zhihu.com/p/24709748)
- [矩阵求导术2](https://zhuanlan.zhihu.com/p/24863977)
