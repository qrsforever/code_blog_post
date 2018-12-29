---

title: 异方差性检验
date: 2018-12-18 22:39:11
tags: [Statistics]
categories: [Books]

---

<!-- vim-markdown-toc GFM -->

* [资源](#资源)
* [数据集](#数据集)
* [Homoscedasticity](#homoscedasticity)
* [Heteroscedasticity](#heteroscedasticity)
    * [Why is it important to check for heteroscedasticity?](#why-is-it-important-to-check-for-heteroscedasticity)
    * [How to detect heteroscedasticity?](#how-to-detect-heteroscedasticity)
        * [1. Graphical Methods](#1-graphical-methods)
        * [2. Statistics Tests](#2-statistics-tests)
    * [How to rectify?](#how-to-rectify)
* [Draft](#draft)
* [实践](#实践)

<!-- vim-markdown-toc -->

<!-- more -->

# 资源

- [同方差](https://www.statisticshowto.datasciencecentral.com/homoscedasticity/)

- [异方差](https://www.statisticshowto.datasciencecentral.com/heteroscedasticity-simple-definition-examples)

- [检测异方差并校准](https://datascienceplus.com/how-to-detect-heteroscedasticity-and-rectify-it/)

- [new online: residuals](https://newonlinecourses.science.psu.edu/stat501/node/277)

- [beginners-guide-regression-analysis-plot][hackerearth]

- [emulating-r-regression-plots-in-python](https://medium.com/@emredjan/emulating-r-regression-plots-in-python-43741952c034)


[hackerearth]: https://www.hackerearth.com/practice/machine-learning/machine-learning-algorithms/beginners-guide-regression-analysis-plot-interpretations/tutorial/

# 数据集

- [百度云](https://pan.baidu.com/s/1zrsRVALr5icPWbxMRWFhSA "Datasets/Statistics/")

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

### 1. Graphical Methods

分析误差项(残差分析)

- Residual vs. Fitted Values Plot(残差散点图): 应该在Y=0, 这条直线上, 随机上下波动, 不会出现U型曲线, 残差相互独立性

- Scale-Location Plot(标准化残差方根散点图): 学生化残差, 使用残差值的方根比残差值更无偏性, (sqrt(|E|)) is much less skewed than | E | for Gaussian zero-mean E), 小于2正常, 如果不开根方的话:标准化残差图, 图中的点一般在-2 ~ 2 之间正常.

- Normality Q-Q Plot(残差QQ图): 倾斜的直线是ok的, 如果发现有曲线U, 说明残差不是正态的, 假定不成立, 残差正态性检验

- Leverage plot(杠杆图): Cookie distance, 库克距离, 是否存在异常数据


### 2. Statistics Tests


## How to rectify?


# Draft

Breush Pagan (布劳殊-培干)


# 实践

{% asset_jupyter python3 notebook/Residuals_Analysis.ipynb %}
