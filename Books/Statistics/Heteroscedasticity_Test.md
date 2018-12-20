---

title: 异方差性检验
date: 2018-12-18 22:39:11
tags: [Statistics]
categories: [Books]

---

<!-- vim-markdown-toc GFM -->

* [资源](#资源)
* [Homoscedasticity](#homoscedasticity)
* [Heteroscedasticity](#heteroscedasticity)
    * [Why is it important to check for heteroscedasticity?](#why-is-it-important-to-check-for-heteroscedasticity)
    * [How to detect heteroscedasticity?](#how-to-detect-heteroscedasticity)
    * [How to rectify?](#how-to-rectify)
* [Draft](#draft)

<!-- vim-markdown-toc -->

<!-- more -->

# 资源

- [同方差](https://www.statisticshowto.datasciencecentral.com/homoscedasticity/)

- [异方差](https://www.statisticshowto.datasciencecentral.com/heteroscedasticity-simple-definition-examples)

- [检测异方差并校准](https://datascienceplus.com/how-to-detect-heteroscedasticity-and-rectify-it/)

# Homoscedasticity

homoscedasticity means "having the same scatter." 

As variance is just the standard deviation squared, you might also see homoscedasticity described as a condition where the standard deviations are equal for all points.

Running a test without checking for equal variances can have a significant impact on your results and may even invalidate them completely.

很多统计检验都假设**等方差**条件, 如果条件不满足, 会产生错误的结果.

# Heteroscedasticity

homoscedasticity means "having the different scatter." where points are at widely varying distances from the regression line.

## Why is it important to check for heteroscedasticity?

在线性回归模型中, 不能用X解释Y的那些部分都放入了误差项(可能还有一些未被发现的因素), 模型的稳健型就看误差项, 如果误差项不是同方差的(比如随着X, 标准误差变动), 那么构建的模型不够稳.

## How to detect heteroscedasticity?

1. Graphical Methods


2. Statistics Tests


## How to rectify?


# Draft

Breush Pagan (布劳殊-培干)
