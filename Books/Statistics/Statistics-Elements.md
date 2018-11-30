---

title: 笔记之统计学基础
date: 2018-11-05 15:28:39
tags: [Statistics]
categories: [Books]

---

<!-- vim-markdown-toc GFM -->

* [参考书籍](#参考书籍)
* [概率论](#概率论)
    * [概念](#概念)
    * [多维](#多维)
    * [条件分布和条件期望](#条件分布和条件期望)
    * [大数定理](#大数定理)
    * [经验分布](#经验分布)
* [数理统计(统计学)](#数理统计统计学)
    * [抽样](#抽样)
    * [参数估计](#参数估计)
    * [假设检验](#假设检验)
* [学习框架](#学习框架)

<!-- vim-markdown-toc -->

# 参考书籍

1. [概率论与数理统计(第二版)-茆诗松](https://pan.baidu.com/s/1YiHtPGpQw8rhfhJ44CbOcQ) 提取码: `b49i`
2. [概率论与数理统计(第四版)-盛骤](https://pan.baidu.com/s/1wef9R6gBj1MLhyvR1rEtmA) 提取码: `812x`
3. [统计学(第四版)-贾俊平](https://pan.baidu.com/s/1lhZMOzzaY7z15UWUbKZpQg) 提取码: `553s`
4. [Probability and Statistics (4th)-Morris H. DeGroot](https://pan.baidu.com/s/1aSLWmIdLsdPAwh1Py56PyQ) 提取码: `rupp`

<!-- more -->

# 概率论

研究随机变量, 假设分布是已知的, 研究它的数字特征, 性质, 特点, 规律性

## 概念

P: 区间, 范围, $\leq$, 分布函数

p: 点, 密度函数, P 是 p的积分/累积

什么时候使用X, 什么时候使用x, 一般会发现{}里会出现大写, ()里用小写, 例如下面的边缘分布:

F: 分布, f: 密度

$$
F_X(x) = P\{X \leq x\} = P\{X \leq x, Y \leq \infty\} = F(x, \infty)
$$

-----------------------------------------------------------------

参数空间: $\theta \in \Theta$

基本结果(基础事件): $\omega \in \Omega$ ; 一枚硬币, 一个骰子, 一个人, 一个家庭

样本空间(基本空间): $\Omega = \{ \omega_1, \omega_2, ..., \omega_i \}, \omega_1\ \text{ is a sample point}$

-----------------------------------------------------------------

随机变量: $X = X(\omega)$ , 所有分布函数都是$X \leq x$, 这个变量一定是个数吗?

$$
P(X \in C) = P({e: X(e) \in C})
$$

C: 子集$\in \Omega$

加入一个变量($\mathbf{X,Y,Z}$)在数轴上的取值($x,y,z$)依赖于随机现象的基本结果, 则称此变量为随机变量

随机变量X, 可以理解为一个函数? 将样本空间${e_1,e_2,\cdots}$的$s_x$作为参数$X(e_i) \in C$

随机表现在变量是由随机试验产生的随机事件, 变量说明是实轴上的一个数, 问题是并不是所有的随机事件都是数字来描述, 比如投硬币:正反面$S=\{e\}$, 所以需要"$X = X(e)$ 实数"的对应关系

-----------------------------------------------------------------

分布列(概率分布): $\sum_{i=1}^{\infty}p(x_i)$ , 概率的集合$\{p(x_i)\}$  - 离散

密度函数(概率分布): $P(a \leq X \leq b) = \int_{a}^{b}p(x)dx \longrightarrow p(x)$  - 连续

分布函数(累积函数): $F(x) = P(X \leq x)$

$$
F(x) = \sum\limits_{x_i \leq x}p(x_i) \qquad (离散) \\
F(x) = \int_{-\infty}^{x}p(x)dx \qquad (连续)
$$

数学期望:

$$
E(X) = \sum\limits_{i = 1}^{n}x_ip(x_i) \qquad (离散) \\
E(X) = \int_{-\infty}^{\infty}xp(x)dx \qquad (连续)
$$


泊松分布: 在一定时间内, 某事件发生的次数

指数分布: 首次发生某个事件的时间

变异系数: 以期望为单位度量随机变量的波动程度

## 多维

$\mathbf{X}(\omega) = (X_1(\omega), X_2(\omega), X_3(\omega), X_n(\omega))$

一个人(基本结果/基础事件)的体重, 身高

-----------------------------------------------------------------

联合分布: 多维随机变量的概率分布

$$
F(x_1, x_2, ..., x_n) = P(X_1 \leq x_1, X_2 \leq x_2, ..., X_n \leq x_n)
$$

联合概率密度(联合密度): $f(xy)$

$$
F(x,y) = \int_{-\infty}^x\int_{-\infty}^yf(x,y)dxdy
$$

-----------------------------------------------------------------

边缘分布: 例如二维, $F(x, y)$ 是 "$X \leq x \cap Y \leq y$" 的交事件

$$
\begin{cases} \lim_{y \to \infty}F(x, y) = P(X \leq x, Y < \infty) = F_X(x) = F(x, \infty) = P(X \leq x) \\
\\
\lim_{x \to \infty}F(x, y) = P(X < \infty, Y \leq x) = F_Y(x) = F(\infty, y) = P(Y \leq y) \end{cases}
$$

边缘分布律(离散): $p_{i \cdotp}, p_{\cdotp i}$

边缘密度函数(连续): $p_X(x) ;\  p_Y(y)$ ; 边缘分布公司好理解, 边缘密度函数([]括号里面的)理解上有些困难, 几何意义

$$
\text{边缘分布:} \\
F_X(x) = F(x, \infty) = \int_{-\infty}^x\big[\int_{-\infty}^{+\infty}f(x,y)dy\big]dx \\
\text{边缘密度:} \\
f_X(x) = \int_{-\infty}^{+\infty}f(x,y)dy
$$

-----------------------------------------------------------------

全概率

条件概率

相互独立的随机变量: 父亲和儿子的身高显然不具有独立性, 两人如果投骰子的点数是独立的

$$
\begin{align} F(x_1, x_2, \cdots, x_n) &= F_1(x_1) F_2(x_2) \cdots F_n(x_n) \\ \\
&= P(X_1 \leq x_1) P(X_2 \leq x_2) \cdots P(X_n \leq x_n) \end{align}
$$

$F_n(x_n)$ 是边缘分布

-----------------------------------------------------------------

多维随机变量**函数**的数学期望:

$$
E[g(X,Y)] =
    \begin{cases}
        \sum_i^{\infty}\sum_j^{\infty}g(x_i,y_j)P(X=x_i, Y=y_j), & (离散) \\
        \int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}g(x, y)dxdy, & (连续) \\
    \end{cases}
$$

$g(x,y)$ 可以只关于$x$或者$y$的随机变量的函数.

协方差(相关矩): $g(X,Y)=(X-EX)(Y-EY)$

$$
\begin{align}
Cov(X, Y) = E[g(X, Y)] = E[(X-EX)(Y-EY)] \tag{1}
\end{align}
$$

(线性)相关系数: $Corr(X, Y) = Cov(X, Y) / \delta_x \delta_y$

-----------------------------------------------------------------

马尔科夫不等式, 切比雪夫不等式: [知乎解答](https://www.zhihu.com/question/27821324), 只是对概率的一个估计, 有可能不是很准确, 但比瞎猜好.

-----------------------------------------------------------------

## 条件分布和条件期望

X,Y独立, 揭露他们之间隐含的趋势

条件分布律

条件密度函数 ? 离散|连续

条件密度的均值: 条件期望

条件分布: $P(X = x_i|Y = y_j) = \dfrac{P(X = x_i, Y = y_i)}{P(Y = y_j)} = \dfrac{P_{ij}}{P_{*j}}$

爸爸的身高(Y)对孩子身高(X)的条件分布情况(条件分布), Y越大, X一般也会越大, 条件分布:$E(X|Y = y_j)$

<<概率论与数理统计(第二版)-茆诗松>> **P153** 有一张图很形象

$$
\begin{cases}
    P(X = j | Y = 1) =
        \begin{cases}
            0.2, & \text{j = 1} \\
            0.3, & \text{j = 2} \\
            0.5, & \text{j = 3}
        \end{cases} & \text{离散} \\
    \\ \\
    \begin{eqnarray}
        P(X \leq x | y \leq Y \leq y + \Delta{y})
            &=& \dfrac{P(X \leq x, y \leq Y \leq y + \Delta{y})}{P(y \leq Y \leq y + \Delta{y})} \\ \\
            &=& \dfrac{\int_{-\infty}^{x}\int_y^{y+\Delta{y}}p(x,y)dydx}{\int_y^{y+\Delta{y}}p_Y(y)dy}
    \end{eqnarray} & \text{连续}
\end{cases}
$$


## 大数定理

辛钦大数定理(law larger number): 试验次数很大时, 频率代替概率

> 随机变量$X_1, X_2, \cdots$独立同分布, 且期望$E(X_k) = \mu$, 则: 依概率收敛于

$$
\bar{X} = \dfrac{1}{n}\sum_{k = 1}^nX_k \ \overset{P}{\longrightarrow} \mu
$$

## 中心极限定理

莱维-林德伯格

独立同分布的中心极限定理(central limit theorem): n足够大时, 近似服从正态分布, 大样本统计推断的基础

> iid, 随机变量之**和**或**均值**的分布函数F(x), 当n足够大时, 不管原总体分布如何,  F(x)近似服从正太分布  
>
> 1. **和**: $T_n = X_1 + X_2 + \cdot + X_n \approx \mathcal{N}(n\mu, n\sigma^2)$
>
> 2. **均值**:  $M_n = \sum_{i=0}^{n}/nX_i \approx \mathcal{N}(\mu, \sigma^2/n)$
>
> 一个样本中, 样本点受随机因素影响, 之间相互抵消, 所以样本均值的波动(样本方差)比单个样本点的波动要小($1/\sqrt{n}$)
> 
> 伯努利随机变量的和 -> 二项分布 -> 正态分布

另一种的表述更好:

$$
\dfrac{\bar{X} - \mu}{\sigma/\sqrt{n}} \  \overset{approx}{\sim} \ N(0, 1) \ \text{or} \ \
\bar{X} \ \overset{approx}{\sim} \  N(\mu, \sigma^2/n)
$$

### 代码演示

`{% asset_jupyter python3 asset/Central_Limit_Theory.ipynb %}`

-----------------------------------------------------------------

棣莫佛-拉普拉斯定理 (De Moivre-Laplace)

大样本到底有多大, 才能近似正态?

> 二项分布只要np和n(1-p)都大于5
> 
> 泊松分布λ大于20

-----------------------------------------------------------------

## 经验分布

和样本有关

将样本值顺序排列之后的累积分布, 单调不减, 阶梯函数, 右连续

格里纹科定理:
    当$n\ \to\ \infty$, "曲线"相对平滑, 近似总体分布, $F_n(x) \to F(x)$

# 数理统计

随机变量的分布未知, 多次重复独立观察, 推断它的分布

贾俊平: 统计量在统计学中的地位和随机变量在概率论中的地位一样重要

## 抽样

独立同分布: iid(independent identical distribution)

总体参数如: 均值, 方差, 比例等是常数, $\mu \  \sigma \  \pi$

样本统计量: 统计样本计算出来的, 随机变量, $\bar{x} \  S \  p$, 每次抽样结果可能不同, 所以有抽样分布

样本: $(X_1, X_2, \cdots, X_n)$, 样本值: $(x_1, x_2, \cdots, x_n)$

样本分布函数:

$$
F_*(x_1, x_2, \cdots, x_n) = \prod_i^nF(x_i)
$$

样本密度函数:

$$
f_*(x_1, x_2, \cdots, x_n) = \prod_i^nf(x_i)
$$

样本的函数(统计量): $g(X_1, X_2, \cdots, X_n)$

样本均值, 方差, k阶原点矩, k阶中心矩

(估计量的)抽样分布: 统计量的分布 (重复抽样)


>  目的通过样本提取出总体的相关信息, 样本值杂乱无章, 不方便提取总体信息, 所以需要构造统计量, 通过分析统计量的分布(抽样分布)提取总体信息
如果构造的统计量能够提取出所有的总体信息, 则为充分统计量

-----------------------------------------------------------------

思考这些分布? [张老师漫谈六西格玛](https://zhuanlan.zhihu.com/p/24968531)

- 总体分布
- 样本分布: 经验分布, 当样本量n很大时接近总体分布, 一个样本的数据的频数分布
- 抽样分布: 用来干啥的?, 样本统计量的概率分布

三大抽样分布:

- $\chi^2$-分布 (卡尔·皮尔逊, 假设检验的祖师, 拟合优度) [张老师](https://zhuanlan.zhihu.com/p/25165318)

- $\mathcal{t}$-分布 (戈塞特, 小样本) [张老师](https://zhuanlan.zhihu.com/p/25142205)

- $\mathcal{F}$-分布 (费歇尔Fisher, F的来源, 方差分析, 极大似然估计)

### 代码演示

`{% asset_jupyter python3 asset/Sampe_Distribution.ipynb %}`

奠定了假设检验的基础

-----------------------------------------------------------------

如何正确抽样?

> 如果进一批货N, 抽样多少才算合适呢? 有没有标准?

## 参数估计

函数: (样本)统计量和估计量的区别

> 估计量只是统计量的特殊情况, 应用在估计参数(点估计), 统计量是参数已知的样本函数, 估计量(构造一个的统计量)估计参数值.

估计量(estimator): 点估计,区间估计, 点估计?可靠性? --> 区间估计

- 贾俊平: 用来估计总体参数的统计量 $\hat{\theta}$

抽样标准误差

自信水平

自信区间: 依样本而变, 区间随机, 总体的参数是个常数(不是变量), 但未知. 这个区间要么包含该值, 要么不包含, 不能说以某概率包含


似然函数

## 假设检验

# 学习框架

```





                                                  E(X): expectation
                                                          ^
                                                          |
                                                          |
                                                 F(x): distribution          F(x,y): joint distribution
                                                          ^                                ^
                                                          |                                |
                                                          |                                |
                                                          |                                |
                                                  X:random variable   ------>   multiple random variable  ------> F_Y(y): marginal distribution
                                                          ^                                |
                                                          |    X = X(e)         Y = Y(e)   |
                                                          |                                |
                                    P:probability         |                                v
                                          |               |              F_X|Y(x|y): conditional distribution
                                          |               |
                                    {e}:events -----------+
                                          |
                                          |
                              S = {e...}  | S = {(x,y): range(x), range(y)}
                                          |
                                 +-------------------+
                                 |                   |                          Chi-Square    t      F
                               discrete       continuous                              \       |      /
                                  \                 /                                  \      |     /
                                   \  countable?   /                                    ------------
                                    \             /    idd                                    |
                                   S:sample sapce   -------->  randmom sample  -----> sampling distribution
                                          |                                                   ^
                                          |                                                   |
                                          |                                                   |
                           E:randmom variable experiment                   law larger number  &  central limit theorem




E: 一次随机试验/调查
S: 如果是简单样本空间, 每个事件的概率为1/n
X: 事件包含在样本空间中, 只有在试验结束才能知道结果

```
