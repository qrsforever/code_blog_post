---

title: 统计学基础
date: 2018-11-05 15:28:39
tags: [Statistics]
categories: [Books]

---

<!-- vim-markdown-toc GFM -->

* [参考书籍](#参考书籍)
* [概念](#概念)
* [多维](#多维)

<!-- vim-markdown-toc -->

## 参考书籍

1. [概率论与数理统计(第二版)-茆诗松](https://pan.baidu.com/s/1YiHtPGpQw8rhfhJ44CbOcQ) 提取码: `b49i`
2. [概率论与数理统计(第四版)-盛骤](https://pan.baidu.com/s/1wef9R6gBj1MLhyvR1rEtmA) 提取码: `812x`
3. [统计学(第四版)-贾俊平](https://pan.baidu.com/s/1lhZMOzzaY7z15UWUbKZpQg) 提取码: `553s`

<!-- more -->

## 概念

参数空间: $\theta \in \Theta$

基本结果(基础事件): $\omega \in \Omega$ ; 一枚硬币, 一个骰子, 一个人, 一个家庭

样本空间(基本空间): $\Omega = \{ \omega_1, \omega_2, ..., \omega_i \}$

随机变量: $X = X(\omega)$ , 所有分布函数都是$X \leq x$, 这个变量一定是个数吗?

> 加入一个变量($\mathbf{X,Y,Z}$)在数轴上的取值($x,y,z$)依赖于随机现象的基本结果, 则称此变量为随机变量

分布列(概率分布): $\sum_{i=1}^{\infty}p(x_i)$ , 概率的集合$\{p(x_i)\}$  - 离散

密度函数(概率分布): $P(a \leq X \leq b) = \int_{a}^{b}p(x)dx \longrightarrow p(x)$  - 连续

分布函数(累积函数): $F(x) = P(X \leq x)$

$$
F(x) = \sum\limits_{x_i \leq x}p(x_i) \qquad (离散) \\ 
F(x) = \int_{-\infty}^{x}p(x)dx \qquad (连续)
$$

数学期望: 

$$
E(X) = \sum\limits_{i = 1}^{n}x_ip(x_i) \qquad (离散) \\
E(X) = \int_{-\infty}^{\infty}xp(x)dx \qquad (连续)
$$


泊松分布: 在一定时间内, 某事件发生的次数

指数分布: 首次发生某个事件的时间


## 多维

$\mathbf{X}(\omega) = (X_1(\omega), X_2(\omega), X_3(\omega), X_n(\omega))$

一个人(基本结果/基础事件)的体重, 身高

联合分布: 多维随机变量的概率分布

$$
F(x_1, x_2, ..., x_n) = P(X_1 \leq x_1, X_2 \leq x_2, ..., X_n \leq x_n)
$$

边缘分布: 例如二维, $F(x, y)$ 是 "$X \leq x \cap Y \leq y$" 的交事件

$$
\begin{cases} \lim_{y \to \infty}F(x, y) = P(X \leq x, Y < \infty) = F_X(x) = F(x, \infty) = P(X \leq x) \\
\\
\lim_{x \to \infty}F(x, y) = P(X < \infty, Y \leq x) = F_Y(x) = F(\infty, y) = P(Y \leq y) \end{cases}
$$

联合概率密度(联合密度): $p(xy)$

$$
F(x,y) = \int_{-\infty}^x\int_{-\infty}^yp(x,y)dxdy
$$

边缘密度函数

相互独立的随机变量: 父亲和儿子的身高显然不具有独立性, 两人如果投骰子的点数是独立的

$$
\begin{align} F(x_1, x_2, \cdots, x_n) &= F_1(x_1) F_2(x_2) \cdots F_n(x_n) \\ \\
&= P(X_1 \leq x_1) P(X_2 \leq x_2) \cdots P(X_n \leq x_n) \end{align}
$$

$F_n(x_n)$ 是边缘分布
