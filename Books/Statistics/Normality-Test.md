---

title: 笔记之正态性检验
date: 2018-11-27 13:56:02
tags: [Statistics]
categories: [Books]

---


<!-- vim-markdown-toc GFM -->

* [书籍](#书籍)
* [正态分布](#正态分布)
    * [统计量](#统计量)
    * [代码](#代码)
    * [估计量](#估计量)
* [W检验](#w检验)
* [KS检验](#ks检验)

<!-- vim-markdown-toc -->

# 书籍

[正态性检验社-梁小筠](https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw) 提取码: `u4w4`

<!-- more -->

# 正态分布

## 统计量

矩(monments): 均值, 方差, 偏度(skewness), 峰度(kurtosis)

量刚: 三阶中心矩的量刚是随机变量的立方

正态分布的中心距:

$$
\mu_k = E(X - \mu)^k =
    \begin{cases}
        0, & \text{k 是 奇数} \\
        1*3*5*(k - 1)\sigma^2 & \text{k 是 偶数}
    \end{cases}
$$

在统计中, 如何描述数据分布的倾斜方向, 以及倾斜的程度, 为什么使用偏度这个统计量呢?

偏度(3阶): 标准化的三阶中心距, 随机变量与中心分布的不对称程度

峰度(4阶): 随机变量在均值附近的相对平坦程度或峰值程度 (中国和国际的定义有些差别) 和数字3有关

## 代码演示

`{% asset_jupyter python3 asset/Normality-Test-1.1.ipynb %}`


## 估计量

如何选择好的估计量:

1. 无偏性

   我们期望通过样本计算出的估计量的值(估计值)能在总体未知参数附近**摆动**, 即: $E(\hat\theta) = \theta$

2. 有效性

    无偏估计量很多, 应该选择**摆动**比较小的估计量, 即: $D(\hat\theta_1) \leq D(\hat\theta_2)$ 选择$\hat\theta_1$


# W检验

Shapiro-Wilk test: 是一种基于相关性的算法, 它越接近1就越表明数据和正态分布拟合得越好.

$$
W = \frac{\left( \sum_{i=1}^n a_i x_{(i)} \right)^2}
            {\sum_{i=1}^n (x_i - \bar{x})^2} \
$$

样本量小于50 ?? (N < 2000) ?? 有个3-50的表查系数值


# KS检验

又名:D检验

Kolmogorov-Smirnov

(N > 2000)

最直观的想法就是拿样本数据与期望的理论分布进行对比，如果差异不大，则可以认为数据服从正态分布，Kolmogorov的检验方法就是这样的。
为了说明Kolmogorov检验的思想，我们还是要用到上一篇的经验累积概率分布曲线。
