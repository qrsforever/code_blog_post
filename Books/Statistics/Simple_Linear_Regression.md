---

title: 一元线性回归(SLR)
date: 2018-12-20 10:37:00
tags: [Statatistics, Python]
categories: [Books]

---


<!-- vim-markdown-toc GFM -->

* [资源](#资源)
* [实践](#实践)

<!-- vim-markdown-toc -->

<!-- more -->

# 资源

- [Using python statsmodels for OLS linear regression][B1]

- [Introduction to Linear Regression Analysis(5th)-Douglas][B2]

- [数据集-百度云][D1]

[B1]: http://markthegraph.blogspot.com/2015/05/using-python-statsmodels-for-ols-linear.html
[B2]: https://pan.baidu.com/s/16FU_ZwFqw7wxH85Ry59chw "提取码: u4w4"

[D1]: https://pan.baidu.com/s/1zrsRVALr5icPWbxMRWFhSA

# 模型

Population Regression Model: $y = \beta_0 + \beta_1 x + \epsilon$

Sample Regression Model: $y_i = \beta_0 + \beta_1 x_i + \epsilon_i, \quad i = 1,2,\cdots,n$

Simple Linear Regression Model: $\hat{y} = \hat{\beta_0} + \hat{\beta_1} x$

the least - squares criterion is
$$
S(\beta_0, \beta_1) = \sum_{i = 0}^{n} (y_i - \beta_0 - \beta_1 x_i)^2
$$

Residual: $e_i = y_i - \hat{y_i} = y_i - (\hat{\beta_0} + \hat{\beta_1} x),  \quad i = 1,2,\cdots,n$

# 问题

after fit:

1. How well does this equation fi t the data?
2. Is the model likely to be useful as a predictor?
3. Are any of the basic assumptions (such as constant variance and uncorrelated errors) violated, and if so, how serious is this?

# 实践

{% asset_jupyter python3 notebook/Simple_Linear_Regression.ipynb %}
