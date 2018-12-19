---

title: 统计推断
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
    * [假设](#假设)
    * [P值](#p值)
    * [总体检验](#总体检验)
        * [一个总体参数检验](#一个总体参数检验)
        * [两个总体参数检验](#两个总体参数检验)

<!-- vim-markdown-toc -->

# 参考书籍

1. [概率论与数理统计(第二版)-茆诗松][B1]
2. [概率论与数理统计(第四版)-盛骤][B2]
3. [统计学(第四版)-贾俊平][B3]
4. [计量经济学(第三版)-李子奈][B4]
5. [Probability and Statistics (4th)-Morris H. DeGroot][B5]

[B1]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"
[B2]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"
[B3]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"
[B4]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"
[B5]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"

<!-- more -->


# 参数估计

最小二乘法(勒让德)

贝叶斯估计法(贝叶斯)

矩估计法(卡尔·皮尔逊)

极大似然估计(费歇尔)

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

[置信区间是一个范围值，由样本统计量所构造的对总体参数的估计区间。因为它的随机性，不可能从一个给定总体的两个样本产生相同的置信区间。但是如果你重复构造置信区间很多次，得到的置信区间中有一定比例将包含总体参数真值。置信区间中包含总体参数真值的次数所占的比率称为置信水平。](https://zhuanlan.zhihu.com/p/37847495)

### 如何理解

通过一次抽样样本的估计量去估计总体参数, 这个估计到底有多**接近**总体的参数, 一般我们以样本估计值为**中心**画一个区域, 那么总体参数的真值有可能在该区域内, 也可能不在, 我们称这个区域叫置信区间($1-\alpha$), **我们把通过构造一个以样本参数的估计值为中心的区间, 来考察这个区间有多大的可能性包含总体参数真值, 这个检验方法叫参数的置信区间估计**.

-----------------------------------------------------------------

### 单总体均值的估计

统计量: z(正态分布), t(t-分布), 对称分布

$$
\begin{cases} 
    z = \frac{\bar{x} - \mu}{\sigma/\sqrt{n}} \sim \  N(0, 1) &  \text{总体方差已知} \\
    t = \frac{\bar{x} - \mu}{\mathcal{S}/\sqrt{n}} \sim \  t(n-1) &  \text{总体方差未知, 且小样本} \\
    z = \frac{\bar{x} - \mu}{\mathcal{S}/\sqrt{n}} \sim \  N(0, 1) & \text{总体方差未知, 且大样本} \\
\end{cases}
$$

### 单总体方差的估计

统计量: $\chi^2$($\chi^2$-分布), 偏态分布


## 贝叶斯估计


# 假设检验

参数估计使用样本信息估计总体参数, 假设检验则是先假设总体的某个参数的值, 然后判断H0的显著性

本章的参数一般指: T检验统计量

## 原理

基本思想: 概率性质的反正法(根据小概率事件原理: 小概率事件在一次随机试验是几乎不可能发生的)

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

临界值由$\mu = \mu_0$ ($1-\alpha$的自信区间)的U分布决定 $U = \dfrac{\bar{X} - \mu_0}{\sigma/\sqrt{n}} \  \sim \  N(0, 1)$

$$
z = \dfrac{\bar{X} - \mu_0}{\sigma/\sqrt{n}}
$$


拒绝域由H1决定

均值假设检验($\mu$是变量, $\mu_0$是已知量均值):

|类型|H1统计变量|临界值|拒绝域|
|:------:|:---:|:---:|:-----:|
|右边检测| $\mu > \mu_0$ | $\mu_{1-\alpha}$ | $\mu > \mu_{1-\alpha}$ |
|左边检测| $\mu < \mu_0$ | -$\mu_{1-\alpha}$ | $\mu < -\mu_{1-\alpha}$ |
|双侧检测| $\mu \neq \mu_0$ | $\mu_{1-\alpha/2} \quad  -\mu_{1-\alpha/2}$ | $\mu > \mu_{1- \alpha/2} \quad  \mu < -\mu_{1-\alpha/2}$ |


## P值

为什么有P值, 用临界值有什么弊端? P值用来说明什么的?

以前为了计算方便, 通常将统计量标准化转换, 计算z值, 现在计算能力的发展, 容易计算P值, P值是个概率, 和H0假设差距的概率

假设H0是对的, 根据样本计算出来的估计统计量(均值)的实际数据与原假设H0之间不一致的概率, 与$\alpha$对比, P值越小, H0越不靠谱


## 总体检验

### 一个总体参数检验

适用于均值,方差和比率

统计量: $z, t, \chi^2$

$\chi^2$: 来之标准正态总体的n个随机变量的平方之和, $\sum_{i = 0}^{n}(\frac{x_i - \bar{x}}{\sigma})^2$

$$
\begin{alignat}{1}
    z = \frac{\bar{x} - \mu_0}{\sigma/\sqrt{n}} \qquad & \text{大样本均值检验统计量} \\

    t = \frac{\bar{x} - \mu_0}{s_2/\sqrt{n}} \qquad & \text{小样本均值检验统计量} \\

    z = \frac{p - \pi_0}{\sqrt{\frac{\pi_0(1-\pi_0)}{n}}} \qquad & \text{大样本比率检验统计量} \\

    \chi^2 = \frac{(n-1)s^2}{\sigma^2} \qquad & \text{方差检验统计量}

\end{alignat}
$$


```
|
|                                                       样本量
|                                                         |
|                                            大           |               小
|                                          +--------------+------------------+
|                                          |                                 |
|                                          |                                 |
|                                       统计量                           总体方差
|                                         z                                  |
|                                                                已知        |      未知
|                                                                 +----------+--------+
|                                                                 |                   |
|                                                                 |                   |
|                                                               统计量              统计量
|                                                                 z                   t
|
```

### 两个总体参数检验

重温 "抽样分布"

-----------------------------------------------------------------

## 参数检验和非参数检验

假设检验分为参数检验(parametric tests)和非参数检验(nonparametric tests).

Parametric tests assume that the data can be well described by a distribution that is defined by one or more parameters, in most cases by a normal distribution. Nonparametric tests do not depend on the data following a specific distribution.

## 其他

拟合优度检验

- 用来检验一批**分类**数据所来自的总体的分布是否与某种理论分布相一致

- **分类**变量之间的相关性

独立性检验

### 列联分析

离散

### 方差分析

离散 + 连续

### 回归分析

连续 + 连续