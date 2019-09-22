---

title: 牛顿法

date: 2019-09-22 14:35:25
tags: [Guide]
categories: [ML]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/ML/Guide/Newton_method.md)

<!-- vim-markdown-toc GFM -->

* [Description](#description)
* [Applications](#applications)
    * [求方程的根](#求方程的根)
    * [求最优解](#求最优解)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Description

牛顿法的基本思想是利用迭代点$x_0$处的一阶导数(梯度)和二阶导数(Hessen矩阵)对目标函数进行二次函数(非线性)近
似,然后把二次模型的极小点作为新的迭代点,并不断重复这一过程,直至求得满足精度的近似**极小值**.

原理是利用**泰勒公式**展开.

在$x_0$处展开:

$$
f(x) = f(x_0) + f'(x_0)(x - x_0) + \dfrac{f''(x_0)(x - x_0)^2}{2!} + \cdots
    + \dfrac{f^{(n)}(x_0)(x -x_0)^n}{n!} + R_n(x) \label{Newton_Taylor} \tag{1}
$$

# Applications

## 求方程的根

取公式($\ref{Newton_Taylor}$)前两项(线性部分), 并令其等于0:

$$
f(x) \approx f(x_0) + f'(x_0) (x - x_0) = 0 \tag{2}
$$

求解: $f'(x_0) \neq 0$ and $x = x_0 - \dfrac{f(x_0)}{f'(x_0)}$

## 求最优解

取公式($\ref{Newton_Taylor}$)前三项:

$$
f(x) \approx f(x_0) + f'(x_0) (x - x_0) +  \dfrac{f''(x_0)(x - x_0)^2}{2!} \tag{3}
$$

则$min\{f(x)\}$ 转化为: $min\{f(x_0) + f'(x_0) (x - x_0) + \dfrac{f''(x_0)(x - x_0)^2}{2!}\} \label{4} \tag{4}$

即对($\ref{4}$)二次函数(抛物线函数)求最小值, 对($\ref{4}$)求导, 并另其等于0(切线斜率为0, 函数的极值点):

$$
f'(x_0) + f''(x_0)(x-x_0) = 0 \Rightarrow x = x_0 - \dfrac{1}{f''(x_0)} f'(x_0) \label{5} \tag{5}
$$

上面公式中$\dfrac{1}{f''(x_0)}$为牛顿迭代每次的步长.

# 迭代图

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/newton_iteration.gif){.center}

# References

- <https://en.wikipedia.org/wiki/Newton%27s_method>
- <https://zh.wikipedia.org/zh-hans/%E7%89%9B%E9%A1%BF%E6%B3%95>
- <https://blog.csdn.net/qq_36330643/article/details/78003952>
- <https://blog.csdn.net/google19890102/article/details/41087931>
