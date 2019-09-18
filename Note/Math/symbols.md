---

title: 数学符号表

date: 2019-06-25 09:53:18
tags: [Math]
categories: [Note]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/Tools/Math/symbols.md)

<!-- vim-markdown-toc GFM -->

* [统计学](#统计学)
* [数学操作符](#数学操作符)
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

# 数学操作符

符号  |  描述  | 例子
------|--------|------
$A \bullet B, A \cdot B$ | operation: dot product |
$A \ast B$ | operation: convolution |
$A \times B$ | operation: cross product | (1,2,5) $\times$ (3,4,−1) = (−22, 16, − 2)
$A \odot B, A \circ B$ | operation: Hadamard product, element-wise multiply |
$A \otimes B$ | operation: Kronecker product, tensor product | {1, 2, 3, 4} $\otimes$ {1, 1, 2} = { {1, 1, 2}, {2, 2, 4}, {3, 3, 6}, {4, 4, 8} }

# 机器学习

符号  |  描述
------|-------
$N, c$ | the total number of training examples, number of classes of output
$x^{(n)}, y^{(n)}$ | the superscript ${(n)}$ denote the variable for the $n^{th}$ individual training example
$x^{[l]}, w^{[l]}, a^{[l]}$ | the superscript ${[l]}$ denote the $l^{th}$ layer, can drop $[]$ on single training example
$z^{x,l}, a^{x,l}$ | the z, activition in the $l^{th}$ on the $x^{th}$ training example
$n_H^{[l]},n_W^{[l]},n_C^{[l]}$ | the numbers of height, width, channel in the $l^{th}$ layer
$x_i, y_i, b_i$ | the subscript $i$ denote the $i^{th}$ elem of the vector $\mathbf{x, y, b}$
$w^l_{jk}$ | weights for connections from the $k^{th}$ neuron in the $(l-1)^{th}$ layer to the $j^{th}$ neuon in the $l^{th}$
$b^l_j$ | the the bias of the $j^{th}$ neuron in the $l^{th}$ layer
$a^l_j$ | the activation of the $j^{th}$ neuron in the $l^{th}$ layer
$\sigma\left(\right)$ | the activation function(or vectorized activation function)
$\mathbf{z}^l = \mathbf{w}^l a^{l-1}+\mathbf{b}^l$ | the intermediate quantity(vector) called weighted input
$\begin{eqnarray} a^{l}_j = \sigma\left( \sum_k w^{l}_{jk} a^{l-1}_k + b^l_j \right)\end{eqnarray}$ | the the activation $a^{l}_j$ in the  in component form
$\begin{eqnarray} a^{l} = \sigma(w^l a^{l-1}+b^l) = \sigma(z^l)\end{eqnarray}$ | the the activation $a^{l}$ in a matrix form
$Cost(), C, Loss(), L, Error(), E$ | the cost function
$\partial C / \partial w, \partial C / \partial b$ | the partical derivatives that cost function with respect to any weights w and bias b in the network
$y = y(x), t = t(x)$ | the corresponding desired ouput over the individual input x
$s \odot t$ | element wise product of the two vectors
$\sigma^l_j = \dfrac{\partial C}{\partial z^l_j}$ | the error of the $j^{th}$ neuron in the $l^{th}$ layer
$\nabla_a C$ | vector of the partial dervatives $\partial C / \partial a^L_j$, the rate of change of C with respect to the output activations
$\begin{eqnarray} \delta^L = \nabla_a C \odot \sigma'(z^L) \end{eqnarray}$ | the error of the ouput layer in matrix-based form
$\begin{eqnarray} \delta^L = (a^L-y) \odot \sigma'(z^L) \end{eqnarray}$ | error in the case of the quadratic cost
$\begin{eqnarray} \delta^l = ((w^{l+1})^T \delta^{l+1}) \odot \sigma'(z^l) \end{eqnarray}$ | the error of nuerons in other layer out of the last layer
$\begin{eqnarray} \dfrac{\partial C}{\partial w^l_{jk}} = a^{l-1}_k \delta^l_j \end{eqnarray}$ |  the rate of change of the cost with respect to any weight
$\begin{eqnarray} \dfrac{\partial C}{\partial w^l} =\delta^l (a^{l-1})^T \end{eqnarray}$ |  matrix-based form: the rate of change of the cost with respect to any weight
$\begin{eqnarray} \dfrac{\partial C}{\partial b^l_j} = \delta^l_j \end{eqnarray}$ |  the rate of change of the cost with respect to any bias in the network
$\begin{eqnarray} \dfrac{\partial C}{\partial b} = \delta \end{eqnarray}$ | matrix-based form: the rate of change of the cost with respect to any bias in the network


[wikipedia](https://en.wikipedia.org/wiki/List_of_mathematical_symbols)
[neuralnetworksanddeeplearning](http://neuralnetworksanddeeplearning.com/chap2.html "recommend")
[1-math.stackexchange.com](https://math.stackexchange.com/questions/52578/symbol-for-elementwise-multiplication-of-vectors/52581#52581?newreg=dcf5afd2e7e042b59fe76f9fe5957510)
[2-math.stackexchange.com](https://math.stackexchange.com/questions/20412/element-wise-or-pointwise-operations-notation/601545#601545)
