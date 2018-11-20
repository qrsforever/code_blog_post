---

title: 笔记之统计推断
date: 2018-11-19 10:38:12
tags: [Statistics]
categories: [Books]

---

<!-- vim-markdown-toc GFM -->

* [参考书籍](#参考书籍)
* [参数估计](#参数估计)
    * [矩估计](#矩估计)
    * [极大似然估计(MLE)](#极大似然估计mle)
    * [区间估计](#区间估计)
    * [贝叶斯估计](#贝叶斯估计)
* [假设检验](#假设检验)
    * [原理](#原理)
    * [拒绝域](#拒绝域)
    * [选择H0H1](#选择h0h1)
    * [P值](#p值)

<!-- vim-markdown-toc -->

# 参考书籍

1. [概率论与数理统计(第二版)-茆诗松](https://pan.baidu.com/s/1YiHtPGpQw8rhfhJ44CbOcQ) 提取码: `b49i`
2. [概率论与数理统计(第四版)-盛骤](https://pan.baidu.com/s/1wef9R6gBj1MLhyvR1rEtmA) 提取码: `812x`
3. [统计学(第四版)-贾俊平](https://pan.baidu.com/s/1lhZMOzzaY7z15UWUbKZpQg) 提取码: `553s`
4. [Probability and Statistics (4th)-Morris H. DeGroot](https://pan.baidu.com/s/1aSLWmIdLsdPAwh1Py56PyQ) 提取码: `rupp`

<!-- more -->


# 参数估计

## 矩估计

样本矩估计总体矩, 不需要知道总体的分布, 几个未知参数, 构造几个$A_k$, 解方程组.

使用$A_k$估计$\mu_k$:  $\hat{\mu_k} = A_k$

$$
A_k = \frac{1}{n}\sum_{i=1}^nx_i^k
$$

方法(k个位置参数):

1. 矩方程组: $\mu_j = g_j(\theta_1, \theta_2, \cdots, \theta_k)$

2. 解方程组: $\theta_j = h_j(\mu_1, \mu_2, \cdots, \mu_k)$

3. 样本估计: $\hat{\theta_j} = h_j(A_1, A_2, \cdots, A_k)$

4. 样本值带入计算

优点: 简单,总体分布可以不知道
缺点: 不唯一, 受样本异常值影响大, 不稳健

## 极大似然估计(MLE)

Book:概率论与数理统计(第二版)-茆诗松, 例5.3.1, 黑白球

思想:

> 总体分布已知, 参数$\theta$可能的取值, 取使样本观测值的概率最大的$\theta$作为$\hat\theta$估计

容量为n的样本, n个观测值出现的联合分布$\prod_{i = 1}^np(x_i; \theta)$

似然函数(关于参数的函数):

$$
    L(\theta) = \prod_{i=1}^np(x_i, \theta)
$$

极大似然:

$$
    L(\hat{\theta}) = \underset{\theta \in \Theta}{max}L(\theta)
$$

## 区间估计

## 贝叶斯估计


# 假设检验

参数估计使用样本信息估计总体参数, 假设检验则是先假设总体的某个参数的值, 然后判断H0的显著性

本章的参数一般指: T检验统计量

## 原理

## 拒绝域

拒绝域$W$, 接受域$A$

如果给定了总体的分布以及显著性水平$\alpha$, 则拒绝域就可以定了.

## 假设

H0: (1)普遍被认为的道理  (2)拒绝它后果很严重(比如, 药效)

H1: (1)研究人员想要证明的

例如:

- H0: 运动健康
- H0: 这个药无效
- H1: 新方法改善效率

一般**H0**没有$\neq$的假设, 但可以含有等号"$\leqslant$, $\geqslant$"

临界值由$\mu = \mu_0$的U分布决定 $U = \dfrac{\bar{X} - \mu_0}{\sigma/\sqrt{n}} \quad \sim \quad N(0, 1)$

拒绝域由H1决定


## P值

为什么有P值, 用临界值有什么弊端? P值用来说明什么的?
