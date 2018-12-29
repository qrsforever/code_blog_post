---

title: OLS, WLS, GLS
date: 2018-12-18 15:16:23
tags: [Statistics]
categories: [Books]

---

<!-- vim-markdown-toc GFM -->

* [资源](#资源)
* [线性回归模型](#线性回归模型)
    * [高斯马尔科夫定理](#高斯马尔科夫定理)
* [OLS: Ordinal Least Squares: 普通](#ols-ordinal-least-squares-普通)
* [WLS: Weighted Least Squares: 加权](#wls-weighted-least-squares-加权)
* [GLS: General Least Squares: 广义](#gls-general-least-squares-广义)
* [实践](#实践)

<!-- vim-markdown-toc -->

# 资源

- [加权最小平方法及稳健回归模型之简介][B1]

- [Introduction to Linear Regression Analysis(5th)-Douglas][B2]

- [OLS in Matrix Form][B3]

[B1]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"
[B2]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"
[B3]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"


<!-- more -->

# 线性回归矩阵形式

## 回顾:协方差与协方差矩阵

方差: $Var(X) = E[(X - E[X])(X - E[X])]$

协方差: $Cov(X, Y) = E[(X - E[X])(Y - E[Y])]$


-----------------------------------------------------------------

总体:$y = X\beta + \epsilon$

Assumptions:

1. $E(\epsilon) = 0$
2. $Var(\epsilon) = \sigma^2\mathbf{I}$

推到：

1. $\epsilon = y - X\hat{\beta}$

2. $\epsilon^T \epsilon = (y - X\hat{\beta})^T (y - X\hat{\beta})$, 求最小

3. $\hat{\beta} = (X^T X)^{-1}X^Ty$ 其中$X^T X$是K x K的矩

-----------------------------------------------------------------

## 高斯马尔科夫定理

在线性回归模型中，如果误差满足零均值、同方差且互不相关(并没要求正态分布或iid)，则回归系数的最佳(更小的方差)线性无偏估计(BLUE, Best Linear unbiased estimator)就是普通最小二乘法估计。

-----------------------------------------------------------------

## 残差的协方差矩阵如何理解

疑点: 对于n个观测点, 哪来的残差矩$M_{n \times n}$

{% blockquote stats, stackexchange https://stats.stackexchange.com/questions/288958/ols-variance-covariance-matrix-of-residuals OLS: Variance Covariance matrix of residuals %}

How do I obtain the variance for a variable with one observation? The same goes for the off-diagonal elements: How come there exists a covariance for 2 single observations?

{% endblockquote %}

-----------------------------------------------------------------


# OLS: Ordinal Least Squares: 普通

同方差(误差项的方差是常数)

$\hat{\beta} = (X^T X)^{-1} X^T y$

# WLS: Weighted Least Squares: 加权

异方差(误差项的方差是变化的)

2. $Var(\epsilon) = \sigma^2\mathbf{V}$;  $V_{n \times n}$ is diagonal but with unequal diagonal elements

# GLS: General Least Squares: 广义

不仅仅是异方差

$\hat{\beta} = (X^T V^{-1} X)^{-1} X^T V^{-1} y$

# 实践
