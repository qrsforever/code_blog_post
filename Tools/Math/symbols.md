---

title: 数学符号表

date: 2019-06-25 09:53:18
tags: [Math]
categories: [Tools]

---

<!-- vim-markdown-toc GFM -->

* [统计学](#统计学)
* [机器学习](#机器学习)

<!-- vim-markdown-toc -->

<!-- more -->

# 统计学


符号 | 描述
-----|-----
$\mathbf{R}$ | 实数集
$\mathbf{R^n}$ | n维实数向量空间, n维欧式空间
$H$ | 希尔伯特空间
$X$ | 输入空间
$Y$ | 输出空间
$x \in X$ | 输入, 实例
$y \in Y$ | 输出, 标记
$\mathit{X}$ | 输入随机变量
$\mathit{Y}$ | 输出随机变量
$T = {(x_1,y_1), (x_2,y_2), \cdots, (x_N,y_N)}$ | 训练数据集
$N$ | 样本容量
$(x_i,y_i)$ | 第i个训练数据点
$x = (x^{(1)}, x^{(2)}, \cdots, x^{(n)})^T$ | 输入向量, n维实数向量
$x_i^{(j)}$ | 输入向量$x_i$的第j个分量
$\mathit{P(X), P(Y)}$ | 概率分布
$\mathit{P(X,Y)}$ | 联合概率分布
$\mathbf{F}$ | 假设空间
$f \in \mathbf{F}$ | 模型, 特征函数
$\theta, \omega$ | 模型参数
$\omega = (\omega_1, \omega_2, \cdots, \omega_n)^T$ | 权值向量
$b$ | 偏置
$J(f)$ | 模型复杂度
$R_emp$ | 经验风险或经验损失
$R_exp$ | 风险函数或期望损失
$L$ | 损失函数, 拉格朗日函数
$\eta$ | 学习率
$\begin{Vmatrix} x \end{Vmatrix}_1$ | $L_1$范数 = $\sum_{i=1}^{N}|x_i|$
$\begin{Vmatrix} x \end{Vmatrix}_2$ | $L_2$范数 = $\sqrt{\sum_{i=1}^{N}x_i^2}$
$(x \cdot {x}')$ | 向量$x$与${x}'$內积
$\mathit{H(X), H(p)}$ | 熵
$\mathit{H(Y|X)}$ | 条件熵
$\mathbf{S}$ | 分离超平面
$\alpha = (\alpha_1, \alpha_2, \cdots, \alpha_n)^T$ | 拉格朗日乘子, 对偶问题变量
$\alpha_i$ | 对偶问题的第i个变量
$K(x, z)$ | 核函数
$sign(x)$ | 符号函数
$I(x)$ | 指示函数
$Z(x)$ | 规范化因子

# 机器学习

符号  |  描述
------|-------
$N$ | 训练总组数(training examples)
$c$ | 分类数classes
$x_i$ | 变量x(向量|数组)的第$i^{th}$项
$\mathbf{W}^{[l]}, W^{[l]}_i, b^{[l]}_i$ | 第l层对象的神经元的参数, l层第$i^{th}$神经元的权重和偏袒
$x^{(n)}$ | 则第n组的预测训练数据输入
$y_k^{(n)}, t_k^{(n)}$ | 分别表示第n组数据计算的第$k^{th}$分量输出值(最后一层神经元), 对应的目标值(target)
$y_i^{(n)}$ | 第n组训练数据输出的第$i^{th}$分量(神经元)
$\{x^{(n)}\}$ | 全部训练数据输入集合或向量
$n_H^{[l]},n_W^{[l]},n_C^{[l]}$ | 分别表示第$l^{th}$层的宽,高,通道数目
$\{E^{(n)}\}, E^{(n)}, E$ | 总损失函数, 特定第n组训练数据的损失函数
$\mathbf{z}^{[l]} = \mathbf{W}^{[l]} \mathbf{x}^{[l]} + b^{[l]}$ | 第$l^{th}$的响应变量向量(对前一层或输入层的信号)
$a_j^{[l]}$ | 第$l^{th}$层第$j^{th}$神经元的激活变量 $\begin{eqnarray} a^{[l]}_j = f\left( \sum_k w^{[l]}_{jk} a^{[l-1]}_k + b^{[l]}_j \right) \end{eqnarray}$
$f^{(l)}_a(z)$ | 第l^{th}层激活函数: sigmoid, tanh, relu...
