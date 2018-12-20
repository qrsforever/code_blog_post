---

title: 异常值和高杠杆率点
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

## 1. 异常值(Outlier)

An outlier is a data point whose response y does not follow the general trend of the rest of the data.

该观测值偏离预测的Y值

## 2. 杠杆率(Leverage)

A data point has high leverage if it has "extreme" predictor x values. 

该观测值的X非常偏离其他X数据


# 实践

{% asset_jupyter python3 notebook/Outlier_and_Leverage.ipynb %}

