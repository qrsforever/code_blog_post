---

title: 回归分析
date: 2018-11-26 19:37:44
tags: [Statistics, Draft]
categories: [Books]

---


<!-- vim-markdown-toc GFM -->

* [资源](#资源)
* [概念](#概念)
    * [函数关系和相关关系](#函数关系和相关关系)
    * [相关分析和回归分析](#相关分析和回归分析)
    * [共线性](#共线性)
    * [虚拟变量](#虚拟变量)
    * [回归](#回归)
    * [拟合优度](#拟合优度)
    * [参考](#参考)
* [回归模型的参数估计](#回归模型的参数估计)
    * [点估计](#点估计)
        * [普通最小二乘OLS](#普通最小二乘ols)
        * [极大似然估计ML](#极大似然估计ml)
        * [矩法MM](#矩法mm)
    * [区间估计](#区间估计)
        * [置信区间估计](#置信区间估计)
        * [预测区间估计](#预测区间估计)
* [回归模型的显著性检验](#回归模型的显著性检验)
    * [线性关系F检验](#线性关系f检验)
    * [回归系数t检验](#回归系数t检验)
    * [预测](#预测)
    * [残差分析](#残差分析)
* [线性回归几种方法实现](#线性回归几种方法实现)
* [其他](#其他)
* [问题](#问题)
* [draft](#draft)

<!-- vim-markdown-toc -->

<!-- more -->

# 资源

- [统计学(第六版)-贾俊平][B1]
- [计量经济学(第三版)-李子奈][B2]

[B1]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"
[B2]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"

# 概念

## 函数关系和相关关系

1. 函数关系

> 圆的的面积, 能量的物理公式等, 一一对应

2. 相关关系

> 身高和体重之间存在关系, 不一一对应

## 相关分析和回归分析

1. 相关分析

> 统计分析方法, 随机变量之间的相关关系, 变量对等, 侧重于变量间相关程度的度量

2. 回归分析

> 统计分析方法, 随机变量之间的依赖关系, 变量不对等, 侧重于Y和X的数量关系, X预测Y. X为可被控制/设定的普通变量

它所关心的是解释变量在给定确定值的情况下, 与之相关被解释变量所有可能出现的对应值的**平均值**.

思考: X确定之后的Y的条件概率分布是确定的, 可以得到Y的条件期望:$E(Y|X)$. (抽样分布)

## 共线性

1. 多重共线性

> 自变量之间存在相关关系(有些打酱油的自变量), 即某自变量可以由其他自变量推导出来, 会导致模型误差变大, 需要剔除

2. 完全共线性

> 某自变量是其他自变量的线性函数, 即可以线性组合出该变量

## 虚拟变量

> TODO, 一般是离散变量, 离散变量经过量化, 哑变量, 使模型更复杂, 但描述更简单

## 回归

1. 线性回归

总体回归函数(PRF): $E(Y|X) = f(X) = \beta_0 + \beta_1X$; 总体回归随机形式(个体): $Y = E(Y|X) + \mu = \beta_0 + \beta_1X + \mu$, 其中$\mu$是随机误差项(干扰项), 非系统部分

样本回归函数(SRF): $\hat{Y} = f(X) = \hat{\beta_0} + \hat{\beta_1}X$; 样本回归随机形式: $Y = \hat{Y} + \hat{\mu} = \hat{\beta_0} + \hat{\beta_1}X + e$, 其中$e$是$\mu$的估计$\hat{\mu}$, **样本残差**.

子曰: (多元)总体回归函数是确定的, 但未知, 所以PRF中的随机误差项$\mu$比SRF中的样本残差$e$少了一些未知解释变量等.

2. 非线性回归

> TODO

3. 广义线性回归

> 非线性回归可转换为线性回归分析, 比如 Logistic回归

## 拟合优度

指回归直线对观测数据的接近程度, 使用判定系数度量.

什么是判定系数$R^2$? 它和相关系数$r$有什么联系和区别?

子曰: 在一元线性回归中: $r = \sqrt{R^2}$, r,$R^2$都可以描述回归直线的拟合程度, 但是r(eg:0.5)的值总是大于$R^2$(eg:0.25), eg: 表面上r看相关程度接近了一半, 实际上只能解释总变差的0.25.

-----------------------------------------------------------------

什么是变差? 它和误差有什么区别? (误差和残差是一个东西吗?)

子曰: 变差是被解释变量的波动,包括x的引起的, 和非x的外因引起的; 残差是样本回归模型的非系统的那部分; 误差更多的是指总体回归模型的非系统的那部分, 但很多地方残差和误差都是一个东西.

变差: $y-\bar{y}$, 观测值与均值的离差

-----------------------------------------------------------------

总变差平方和SST: $SST = \sum(y_i - \bar{y})^2$

回归平方和SSR: $SSR = \sum(\hat{y_i} - \bar{y})^2$; 为什么? $\bar{y}$一条常数直线, $\hat{y_i}$到该直线的距离, 只受到X变化的影响, 公式中没有真实值, 假定都在回归线上.

残差平方和SSE: $SSE = \sum(y_i - \hat{y_i})^2$; 为什么? 样本回归模型非系统的那部分, X设定后, 回归值则定, 真实值与回归值的离差, 回归不能解释的那部分.

SST = SSR + SSE

判定系数: $R^2 = \dfrac{SSR}{SST} = 1 - \dfrac{SSE}{SST}$

-----------------------------------------------------------------

估计标准误差: $s_e = \sqrt{\dfrac{SSE}{n-2}} = \sqrt{MSE}$, 越小拟合越好

-----------------------------------------------------------------

1. 过拟合

> TODO

2. 欠拟合

> TODO

## 参考

- [回归分析之理论篇](https://blog.csdn.net/Gamer_gyt/article/details/78008144)

# 回归模型的参数估计

参数估计要依据样本数据做支撑.

一元

## 点估计

### 普通最小二乘OLS

二乘: 中国把平方和,一般叫做二乘

原理: 从总体随机抽取n组样本观察值, 最合理的参数估计量应该是使模型更好的拟合样本数据

实际**观察值**和通过回归模型计算出的被解释变量**Y**的估计值之差(e)的平方和最小.

即: $e = Y - \hat{Y} = Y - (\hat{\beta_0} + \hat{\beta_1}X); Q=\sum_{i=0}^{n}e_i^2$

通过微积分运算(偏导)计算未知参数.

### 极大似然估计ML

更本质地揭示通过样本数据估计总体参数的内在机理

原理: 从总体随机抽取n组样本观察值, 最合理的参数估计量应该是从模型中抽取这n组样本观察值的概率最大.

### 矩法MM

TODO

了解更多:[计量经济学(第三版)-李子奈][B2]

## 区间估计

-----------------------------------------------------------------

The difference between a prediction interval and a confidence interval is the standard error.

The standard error for a confidence interval on the mean takes into account(考虑) the uncertainty due to sampling. The line you computed from your sample will be different from the line that would have been computed if you had the entire population, the standard error takes this uncertainty into account.

The standard error for a prediction interval on an individual observation takes into account the uncertainty due to sampling like above, but also takes into account the variability of the individuals around the predicted mean. The standard error for the prediction interval will be wider than for the confidence interval and hence the prediction interval will be wider than the confidence interval.

[参考](https://stats.stackexchange.com/questions/16493/difference-between-confidence-intervals-and-prediction-intervals)

-----------------------------------------------------------------

A prediction interval is an interval associated with a random variable yet to be observed, with a specified probability of the random variable lying within the interval. For example, I might give an 80% interval for the forecast of GDP in 2014. The actual GDP in 2014 should lie within the interval with probability 0.8. Prediction intervals can arise in Bayesian or frequentist statistics.

A confidence interval is an interval associated with a parameter and is a frequentist concept. The parameter is assumed to be non-random but unknown, and the confidence interval is computed from data. Because the data are random, the interval is random. A 95% confidence interval will contain the true parameter with probability 0.95. That is, with a large number of repeated samples, 95% of the intervals would contain the true parameter.

[参考](https://robjhyndman.com/hyndsight/intervals/)

-----------------------------------------------------------------

另外: 贝叶斯置信区间(credable interval)

### 置信区间估计

给定$x_0$, 求$y$平均值的区间估计, 对总体参数的估计(non-random but unkown), (重复抽样)我们有信心认为有$100*(1-\alpha)$个样本包含总体参数的真值.

它的误差项: 随机抽样带来的.

公式: $E(Y|X) = \beta_0 + \beta_1X$

### 预测区间估计

给定$x_0$, 求$y$的这个个值的区间估计, 对一个随机(random variable)变量的估计.

它的误差项: 随机抽样+个体的差异

公式: $Y = \beta_0 + \beta_1X + e$

# 回归模型的显著性检验

拟合优度检验

在重复抽样过程中, 参数估计量的期望值会接近总体参数, 但在一次抽样中, 计算出的估计量的值到底与总体参数有多接近呢?

## 线性关系F检验

F分布: 为什么? TODO

一元:

MSR --> SSR/1 --> $\hat{y} - \bar{y}$ --> $\hat{y}$ 是什么分布?

MSE --> SSE/n-2 --> $y - \hat{y}$ 是什么分布?


## 回归系数t检验

在一元回归检验中, 线性关系检验和回归系数检验是等效的, 只有一个系数, 但在多元回归中, 线性关系检验显著, 不代表回归系数检验显著, 因为只要有一个解释变量和被解释变量有线性关系, F检验显著, 并不能说明每个解释变量与被解释变量都是关系显著.


了解更多:[统计学(第六版)-贾俊平][B1]

-----------------------------------------------------------------

## 预测


## 残差分析

residual analysis

线性回归的几个假设前提: 残差等方差正态分布, 相互独立等.

什么时候进行残差分析? 回归模型之前, 显著性检验之后?


# 线性回归几种方法实现

`{% asset_jupyter python3 notebook/Regression_Analysis_Methods.ipynb %}`

# 其他

[八种线性回归方法对比](https://github.com/tirthajyoti/Machine-Learning-with-Python/blob/master/Linear_Regression_Methods.ipynb)

[StatsModels:Regression Plots](https://www.statsmodels.org/dev/examples/notebooks/generated/regression_plots.html)

# 问题

1. <<计量统计学-李子奈>>: 采用OLS已经保证模型最好的拟合样本观测值, 为什么还要进行检验拟合优度呢?

> 书答: OLS是同一个问题内部的比较, 拟合优度检验结果所表示的优劣是不同问题之间的比较. (**尚不理解**)

> 自曰: 在做回归分析时, 我们先去**猜**了, 用OLS和样本数据去回归我们猜的模型, 也许每个人猜的模型不一样, OLS计算出来的估计参数不同, 哪个更好呢.

# draft

ME: 线性回归是不是就是把总体看做线性分布, 和均匀分布/指数分布等一样, 含有参数, 回归分析就是用样本去估计这些参数

Fisher变换 和 相关系数有什么关系?

皮尔逊积矩相关系数 和 斯皮尔曼秩相关系数 的区别? 后者因为前者的什么不足才出现的? 前者鲁棒性差? 为什么有这么多假设?

斯皮尔曼秩相关系数的非参数?

散点图能看出变量间的关系, 但是无法反应之间的强度

r是变量之间的线性关系的度量, 但不一定有因果关系

相关系数也是个**检验统计量**, 既然是检验统计量, 如何检验的?

相关系数根据总体计算出的, 总体相关系数$\rho$, 根据样本计算出来的, 样本相关系数$r$

总体$\rho$是未知的, 样本$r$受随机抽样的波动, $r$是个随机变量, 研究的目的: 样本的$r$能否说明总体$\rho$
