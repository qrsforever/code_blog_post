---

title: 离群点和高杠杆率点
date: 2018-12-20 16:00:28
tags: [Statatistics]
categories: [Books]

---

<!-- vim-markdown-toc GFM -->

* [资源](#资源)
* [概念](#概念)
    * [1. 异常值(Outlier)](#1-异常值outlier)
    * [2. 杠杆率(Leverage)](#2-杠杆率leverage)
* [实践](#实践)

<!-- vim-markdown-toc -->

<!-- more -->

# 资源

- [Influential Points][B1]

[B1]: https://newonlinecourses.science.psu.edu/stat501/node/336/

# 概念

观察点是Outlier Point, 不一定就是Influence Point

观察点是High Leverage Point, 也不一定是Influence Point

{% blockquote newonlinecourses https://newonlinecourses.science.psu.edu/stat501/node/337/ Distinction Between Outliers & High Leverage Observations %}

A data point is influential if it unduly influences any part of a regression analysis, such as the predicted responses, the estimated slope coefficients, or the hypothesis test results.

{% endblockquote %}

是不是Influence Point, 要看包含及排除这个观测点是否对预测Y值和回归模型系数以及统计检验结果有影响.

## 1. 离群点(Outlier)

An outlier is a data point whose response y does not follow the general trend of the rest of the data.

该观测值偏离预测的Y值

## 2. 杠杆率(Leverage)

A data point has high leverage if it has "extreme" predictor x values.

该观测值的X非常偏离其他X数据

推导公式:

$$
\begin{alignat}{1}
Y &= X\beta + \epsilon  \\
\hat{y} &= X(X^{'}X)^{-1}X^{'}y \\
H &= X(X^{'}X)^{-1}X^{'} \\
\hat{y}_i &= h_{i1}y_1+h_{i2}y_2+...+h_{ii}y_i+ ... + h_{in}y_n  \;\;\;\;\; \text{ for } i=1, ..., n \\
\end{alignat}
$$

H是个hat matrix, 为什么叫这个名字? 由$\hat{y} = Hy$, 把y通过H矩阵变换为$\hat{y}$, $h_{ii}$叫**Leverages**, 此值越大说明在$\hat{y}_i$中$y_i$占有更大的角色.

{% blockquote newonlinecourses https://newonlinecourses.science.psu.edu/stat501/node/338/ Definition and properties of leverages %}
Here are some important properties of the leverages:
<ol>
    <li>The leverage hii is a measure of the distance between the x value for the ith data point and the mean of the x values for all n data points. </li>
    <li>The leverage hii is a number between 0 and 1, inclusive. </li>
    <li>The sum of the hii equals p, the number of parameters (regression coefficients including the intercept). </li>
</ol>
{% endblockquote %}

如何通过$h_ii$判断观察点的x值是异常的?

答: 杠杆点均值 $\bar{h} = \dfrac{\sum_{i = 1}^{n}h_{ii}}{n} = \dfrac{p}{n}$, 如果$h_{ii} \gt 3\dfrac{p}{n}$, 则$h_{ii}$是高杠杆率点.

# 实践

{% asset_jupyter python3 notebook/Outlier_and_Leverage.ipynb %}
