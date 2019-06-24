---

title: (原创)交叉熵损失函数

date: 2019-06-23 15:24:43
tags: [Guide]
categories: [ML]

---


<!-- vim-markdown-toc GFM -->

* [百科](#百科)
    * [相关概念](#相关概念)
    * [交叉熵](#交叉熵)
* [交叉熵损失函数](#交叉熵损失函数)
    * [定义符号](#定义符号)
    * [Logistic回归的交叉熵](#logistic回归的交叉熵)
    * [Softmax回归的交叉熵](#softmax回归的交叉熵)
* [参考](#参考)

<!-- vim-markdown-toc -->

<!-- more -->

# 百科

交叉熵(cross entropy)是信息论中的一个重要概念, 度量两个概率分布的差异性信息.

## 相关概念

熵是描述系统的不确定性, 越出乎意料, 越不可能发生的事的信息量越大, 确定的事信息量为0.
"妖是妖他妈生的"信息量为0, "10万在北京买了一套房"的信息量很大(不可能吧)

信息量(self-information) $I(x) = log_2\dfrac{1}{P(x)} = -logP(x)$, 一个不可能的事情的发生比及有可能的事件
发生提供的信息要更多,只能衡量单个事件的信息量. 而整个系统(所有x)呈现出一个分布, 信息熵就是对概率分布进行量
化, 即所有x的自信息的期望. (期望编码长度最小)

两个独立的事件的信息量是可以叠加的.

信息熵$H(X) = E[-log_2p_i] = \sum\limits_{i=0}^{N}P(x_i)log_2\dfrac{1}{P(x_i)} = -\sum\limits_{i=0}^{N}P(x_i)log_2P(x_i)$
代表随机变量或系统的不确定性, 熵越大, 随机变量或系统的不确定性就越大.

简写: $p_i = P(x_i); H(X) = -\sum\limits_{i=0}^{N}p_ilog_2p_i$


KL散度(Kullback Leibler Divergence),有时候也叫KL距离(不同点是不对称),一般被用于计算两个分布之间的不同,散度
越离散值越大, 两个分布相同KL散度为0.
数学定义(离散):
$$
\begin{align*}
D_KL(A||B) &= \sum\limits_{i}P_A(x_i)log_2\bigg(dfrac{P_A(x_i)}{P_B(x_i)}\bigg) \\
           &= \sum\limits_{i}P_A(x_i)log_2\bigg(P_A(x_i)\bigg) - \sum\limits_{i}P_A(x_i)log_2\bigg(P_B(x_i)\bigg) \\
           &= -H(A) + H(A, B)
\end{align*}
$$
如果H(A)是个常数, KL散度$\cong$交叉熵(不严谨), 这也许是**交叉熵**用来作为损失函数的原因.
如果两个分布相同$D_KL(A||B)=0$, 但是$H(A,B)$是不等于0的.
注意: $D_KL(A||B) \neq D_KL(B||A)$


**实际上对数不一定都是以2为底, 10, e都可以, 重要的是确定后, 所有计算都要相同**

## 交叉熵

交叉熵$H(P, Q) = -\sum\limits_{i=0}^{N}p_ilog_2q_i$, 其中$p_i为真实分布, q_i为非真实(预测)分布$, 表示预测分布
与真实分布的错误程度, 如果Q分布越接近P分布, 交叉熵值越小.

连续型: $-\int p(x)logq(x)dx$, 训练的目的就是使q(x)逼近p(x).

实例:

真实分布P: {1/4, 1/4, 1/4, 1/4}

预测分布Q1: { 1/4, 1/2, 1/8, 1/8}

预测分布Q2: { 1/4, 1/4, 1/8, 3/8}

P自信息:
$$
\begin{align*}
H(P, P)  &= -\sum\limits_{i=0}^{n}p_ilog_2q_i \\
         &= -\bigg[1/4 log_2(1/4) + 1/4 log_2(1/4) + 1/4 log_2(1/4) + 1/4 log_2(1/4)\bigg] \\
         &= -\bigg[1/4*(-2) + 1/4*(-2) + 1/4*(-2) + 1/4*(-2)\bigg] \\
         &= 8/4
\end{align*}
$$

P与Q1的交叉熵:
$$
\begin{align*}
H(P, Q1) &= -\sum\limits_{i=0}^{n}p_ilog_2q_i \\
         &= -\bigg[1/4 log_2(1/4) + 1/4 log_2(1/2) + 1/4 log_2(1/8) + 1/4 log_2(1/8)\bigg] \\
         &= -\bigg[1/4*(-2) + 1/4*(-1) + 1/4*(-3) + 1/4*(-3)\bigg] \\
         &= 9/4
\end{align*}
$$

P与Q2的交叉熵:
$$
\begin{align*}
H(P, Q2) &= -\sum\limits_{i=0}^{n}p_ilog_2q_i \\
         &= -\bigg[1/4 log_2(1/4) + 1/4 log_2(1/4) + 1/4 log_2(1/8) + 1/4 log_2(3/8)\bigg] \\
         &= -\bigg[1/4*(-2) + 1/4*(-2) + 1/4*(-3) + 1/4*log_2(3/8)\bigg] \\
         &= \dfrac{10 -log_23}{4} < 9/4
\end{align*}
$$

# 交叉熵损失函数

希望理想模型和真实模型分布一致: $P(model) \cong P(trainning) \cong P(real)$, 根据模型概率分布差异计算预测
模型的可靠性, 真实的模型是无法得知的, 而预测模型也是通过训练数据训练出. KL散度是计算两个事件的概率分布的差
异, 且如果事件A的信息熵$H(A)$固定, 则简化为$H(A, B)$即交叉熵的计算, 当交叉熵最低时(训练数据分布的熵), 得到
最好的模型.


## 定义符号

m: 样本组数

p: 特征个数

n: 分类个数 (多分类: 神经元的个数)

$\theta_j$: 第j个参数

$x^{(i)}$: 第i组数据, $x^{(i)}=(1,x^{(i)}_1,x^{(i)}_2,...,x^{(i)}_p)^T$ 包含偏置

$y^{(i)}$: 第i组数据对应的类标记, $(x^{(i)}, y^{(i)})$代表一组测试/训练样本数据

$\hat{y}^{(i)}$: 模型预测第i组数据的输出的标记

$z^{(i)}$: 第i组数据输入后计算的输出(未sigmoid或softmax) $z^{(i)} = \theta^T x^{(i)} = \sum\limits_j^p\theta_{ij}x_{ij} + b$

## Logistic回归的交叉熵

是非问题, $y^{(i)}$取0或1

0-1问题, 构造假设函数(hypothesis function):
$$
h_\theta(x^{(i)}) = \dfrac{1}{1 + e^{-\theta^T x^{(i)}} }
$$

二分类问题, 设:

$P(\hat{y}^{(i)}=1|x^{(i)};\theta)=h_\theta(x^{(i)})$

$P(\hat{y}^{(i)}=0|x^{(i)};\theta)=1-h_\theta(x^{(i)})$

**不考虑交叉熵, 单纯推导**, 对上面, 取log, 原函数的单调性不变:

$\log P(\hat{y}^{(i)}=1|x^{(i)};\theta)=\log h_\theta(x^{(i)})=\log\frac{1}{1+e^{-\theta^T x^{(i)}} }$

$\log P(\hat{y}^{(i)}=0|x^{(i)};\theta)=\log (1-h_\theta(x^{(i)}))=\log\frac{e^{-\theta^T x^{(i)}}}{1+e^{-\theta^T x^{(i)}} }$

对于第i组样本，假设函数表征正确的组合对数概率为:

$$
\begin{align*}
P &= I\{y^{(i)}=1\}\log P(\hat{y}^{(i)}=1|x^{(i)};\theta)+I\{y^{(i)}=0\}\log P(\hat{y}^{(i)}=0|x^{(i)};\theta) \\
  &= y^{(i)}\log P(\hat{y}^{(i)}=1|x^{(i)};\theta)+(1-y^{(i)})\log P(\hat{y}^{(i)}=0|x^{(i)};\theta) \\
  &= y^{(i)}\log(h_\theta(x^{(i)}))+(1-y^{(i)})\log(1-h_\theta(x^{(i)}))
\end{align*}
$$

其中$I\{y^{(i)}=1\}$ 和 $I\{y^{(i)}=0\}$ 是指示函数, 当{}里成立时为1, 否则为0

m组样本:
$$
\sum_{i=1}^{m}y^{(i)}\log(h_\theta(x^{(i)}))+(1-y^{(i)})\log(1-h_\theta(x^{(i)}))
$$

计算出的值越大越好, 而我们希望损失函数越小越好, 于是:

$$
J(\theta)=-\frac{1}{m}\sum_{i=1}^{m}y^{(i)}\log(h_\theta(x^{(i)}))+(1-y^{(i)})\log(1-h_\theta(x^{(i)}))
$$

代入:

$$
\begin{align*}
J(\theta) &=-\frac{1}{m}\sum_{i=1}^m \left[-y^{(i)}(\log ( 1+e^{-\theta^T x^{(i)}})) + (1-y^{(i)})(-\theta^T x^{(i)}-\log ( 1+e^{-\theta^T x^{(i)}} ))\right]\\
  & =-\frac{1}{m}\sum_{i=1}^m \left[y^{(i)}\theta^T x^{(i)}-\theta^T x^{(i)}-\log(1+e^{-\theta^T x^{(i)}})\right]\\
  & =-\frac{1}{m}\sum_{i=1}^m \left[y^{(i)}\theta^T x^{(i)}-\log e^{\theta^T x^{(i)}}-\log(1+e^{-\theta^T x^{(i)}})\right]\\
  & =-\frac{1}{m}\sum_{i=1}^m \left[y^{(i)}\theta^T x^{(i)}-\left(\log e^{\theta^T x^{(i)}}+\log(1+e^{-\theta^T x^{(i)}})\right)\right] \\
  & =-\frac{1}{m}\sum_{i=1}^m \left[y^{(i)}\theta^T x^{(i)}-\log(1+e^{\theta^T x^{(i)}})\right] 
\end{align*}
$$

计算$J(\theta)$对j个参数分量$\theta_j$求偏导:

$$
\begin{align*}
\frac{\partial}{\partial\theta_{j}}J(\theta) &=\frac{\partial}{\partial\theta_{j}}\left(\frac{1}{m}\sum_{i=1}^m \left[\log(1+e^{\theta^T x^{(i)}})-y^{(i)}\theta^T x^{(i)}\right]\right)\\
   & =\frac{1}{m}\sum_{i=1}^m \left[\frac{\partial}{\partial\theta_{j}}\log(1+e^{\theta^T x^{(i)}})-\frac{\partial}{\partial\theta_{j}}\left(y^{(i)}\theta^T x^{(i)}\right)\right]\\
   & =\frac{1}{m}\sum_{i=1}^m \left(\frac{x^{(i)}_je^{\theta^T x^{(i)}}}{1+e^{\theta^T x^{(i)}}}-y^{(i)}x^{(i)}_j\right)\\
   & =\frac{1}{m}\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}
\end{align*}
$$

二分类交叉熵的形式:

$$
H(p,q)=-\sum _x (p(x)logq(x)+(1-p(x))log(1-q(x)))
$$

## Softmax回归的交叉熵

多分类问题. 柔性最大值. 接收N维向量输出一个(0,1)之间的实数

**交叉熵就是用来判定实际的输出与期望的输出的接近程度**

(神经网络)如果最后的输出节点数为N, 对于每一个输入样本X, 最后得到的输出为N为数组, 且数组中的每一个维度(元素
)对应一个类别(标记), 如果一个样本属于k, 则对应输出数组的元素为1, 其他为0, 例如[0, 0, 1, ..., 0](期望输出),
这个是理想情况下, 真实得到是[0.01, 0.12, 0.7, ...., 0.11](实际输出), 原始输出$[y_1, y_2, ..., y_n]$加上
softmax之后的结果.

softmax函数模型:
$$
a_i = softmax(y_i) = \frac{e^{y_i}}{\sum_{j=0}^{n}e^{y_j}}
$$
其中$y_i = \sum_j\theta_{ij}x_{ij} + b$表示输出结果数组中第i个元素值, n表示分类个数. **注意:这是一个样本**

假设概率分布p为期望输出(标签)，概率分布q为实际输出，H(p,q)为交叉熵. 

多分类交叉熵的形式:

$$
H(p,q)=-\sum _x p(x)log q(x)
$$

便于求导写成(以e为底):

$$
L = -\sum_i y_i ln a_i
$$

求导:

(待补充)


# 参考

[交叉熵损失函数][1]

[Neural Network Cross Entropy][2]

[归一化 信息熵 交叉熵][3]

[为什么交叉熵可以用于计算代价][4]

[交叉熵代价函数(损失函数)及其求导推导][5]

[Softmax][6]


[1]:https://www.jianshu.com/p/b07f4cd32ba6 "jianshu"
[2]:https://visualstudiomagazine.com/Articles/2017/07/01/Cross-Entropy.aspx
[3]:https://www.cnblogs.com/yjmyzz/p/7822990.html "cnblogs"
[4]:https://www.zhihu.com/question/65288314?sort=created "zhihu"
[5]:https://blog.csdn.net/jasonzzj/article/details/52017438 "csdn"
[6]:https://blog.csdn.net/lilong117194/article/details/81542667 "csdn"
