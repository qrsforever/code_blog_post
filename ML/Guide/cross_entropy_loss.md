---

title: (原创)交叉熵损失函数

date: 2019-06-23 15:24:43
tags: [Guide]
categories: [ML]

---

# 百科

交叉熵(cross entropy)是信息论中的一个重要概念, 度量两个概率分布的差异性信息.

## 相关概念

信息量(self-information) $I(x) = log_2\dfrac{1}{P(x)} = -logP(x)$, 一个不可能的事情的发生比及有可能的事件
发生提供的信息要更多,只能衡量单个事件的信息量. 而整个系统(所有x)呈现出一个分布, 信息熵就是对概率分布进行量
化, 即所有x的自信息的期望.


信息熵$H(X) = \sum\limits_{i=0}^{N}P(x_i)log_2\dfrac{1}{P(x_i)} = -\sum\limits_{i=0}^{N}P(x_i)log_2P(x_i)$
代表随机变量或系统的不确定性, 熵越大, 随机变量或系统的不确定性就越大.

简写: $p_i = P(x_i); H(X) = -\sum\limits_{i=0}^{N}p_ilog_2p_i$


**实际上对数不一定都是以2为底, 10, e都可以, 重要的是确定后, 所有计算都要相同**

## 交叉熵

交叉熵$H(P, Q) = -\sum\limits_{i=0}^{N}p_ilog_2q_i$, 其中$p_i为真实分布, q_i为非真实(预测)分布$, 表示预测分布
与真实分布的错误程度, 如果Q分布越接近P分布, 交叉熵值越小.

实例:

真实分布P: {1/4, 1/4, 1/4, 1/4}

预测分布Q1: { 1/4, 1/2, 1/8, 1/8}

预测分布Q2: { 1/4, 1/4, 1/8, 3/8}

$$
\begin{align*}
H(P, Q1) &= -\sum\limits_{i=0}^{n}p_ilog_2q_i \\
         &= -\bigg[1/4 log_2(1/4) + 1/4 log_2(1/2) + 1/4 log_2(1/8) + 1/4 log_2(1/8)\bigg] \\
         &= -\bigg[1/4*(-2) + 1/4*(-1) + 1/4*(-3) + 1/4*(-3)\bigg] \\
         &= 9/4
\end{align*}
$$

$$
\begin{align*}
H(P, Q2) &= -\sum\limits_{i=0}^{n}p_ilog_2q_i \\
         &= -\bigg[1/4 log_2(1/4) + 1/4 log_2(1/4) + 1/4 log_2(1/8) + 1/4 log_2(3/8)\bigg] \\
         &= -\bigg[1/4*(-2) + 1/4*(-2) + 1/4*(-3) + 1/4*log_2(3/8)\bigg] \\
         &= \dfrac{10 -log_23}{4} < 9/4
\end{align*}
$$

## 交叉熵损失函数

# 参考

[交叉熵损失函数][1]

[Neural Network Cross Entropy][2]

[归一化 信息熵 交叉熵][3]


[1]:https://www.jianshu.com/p/b07f4cd32ba6 "简书"
[2]:https://visualstudiomagazine.com/Articles/2017/07/01/Cross-Entropy.aspx
[3]:https://www.cnblogs.com/yjmyzz/p/7822990.html "cnblogs"
