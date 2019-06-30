---

title: (原创)交叉熵实例

date: 2019-06-25 11:33:49
tags: [Demo]
categories: [ML]

---


## 神经网络

### 简图

```
   input layer     hide layer1         hide layer2           ouput layer
                       L-2                 L-1                   L

      N                I                    J                    K

    *******          *******             *******              *******
  **       **      **       **  a(z)   **       **  a(z)    **       **  a(z)
  *   x1    *      *  wx+b   * ------> *  wa+b   * ------>  *  wa+b   *  ----->
  **       **      **       **  relu   **       **  relu    **       **   softmax
    *******          *******             *******              *******


    *******          *******             *******              *******
  **       **      **       **         **       **          **       **
  *   x2    *      *         *         *         *          *         *
  **       **      **       **         **       **          **       **
    *******          *******             *******              *******
       .                .                   .                    .
       .                .                   .                    .
       .                .                   .                    .

    *******          *******             *******              *******
  **       **      **       **         **       **          **       **
  *   xN    *      *   hI    *         *   hJ    *          *   oK    *
  **       **      **       **         **       **          **       **
    *******          *******             *******              *******
                     a_i(z_i)            a_j(z_j)             a_k(z_k)
```

### 描述

神经网络包含1个输入层, 2个隐藏层, 1个输出层.

输出层(神经元)节点数为N, 总层数L, 所以倒数第一个隐藏层为第L-1层, 以此类推L-2层.

第一个隐藏层的节点数为I个, 第二个隐藏层的节点数为J个, 输出层的节点数为K个.

隐藏层使用的激活函数为Relu, 输出层的激活函数我Softmax.

除了输入层外, 每个神经元的输入都是上一层所有节点经过激活函数之后的值, 例如:

输出层的第k个神经元的真实输出$t_k$

L层: 

$$
\begin{align*}
z_i^{L-2} &= \sum_n^N w_{in}^{L-2} x_n + b_i \\
a_i^{L-2} &= relu(z_i^{L-2}) \\
          &= max(0, z_i^{L-2} \\
z_j^{L-1} &= \sum_i^I w_{ji}^{L-1} a_i + b_j \\
a_j^{L-1} &= relu(z_j^{L-1}) \\
          &= max(0, z_j^{L-1} \\
z_k^{L} &= \sum_j^{J} w_{kj}^{L} a_j + b_k  \tag{0} \\
a_k^{L} &= softmax(z_k^{L}) \\
        &= \frac{e^{z_k^{L}}}{\sum_c^K e^{z_c^{L}}}
\end{align*}
$$

交叉熵(误差估计):

$$
\begin{align}
E = -\sum_k^K t_k log a_k^L = -\sum_k^K t_k (z_k^L - log\sum_c^K e^{z_c^L}) \tag{1}
\end{align}
$$

### 梯度

更新权重(组成第k个节点对应上一层第j个节点的参数): $\Delta W \propto =-\frac{\partial E}{\partial W}$

输出层:

$$
\begin{align*}
\Delta W_{kj}^L &= -\eta \frac{\partial E}{\partial W_{kj}} \\
    &= -\eta \frac{\partial E}{\partial a_k^L} \frac{\partial a_k^L}{\partial z_k^L} \frac{\partial z_k^L}{\partial W_{kj}^l} \\
\end{align*}
$$

计算:

$$
\begin{align}
    \frac{\partial E}{\partial W_{kj}^L} = \frac{\partial E}{\partial z_{k}^L} \frac{\partial z_{k}^L}{\partial W_{kj}^L} \tag{2}
\end{align}
$$

$$
\begin{align*}
\frac{\partial E}{\partial z_{k}^L} &= - \sum_{d}^K t_d (\mathbb{1}_{d=k} - \frac{1}{\sum_c e^{z_c^L}} e^{z_k^L}) \nonumber \\
&= - \sum_d^K t_d (\mathbb{1}_{d=k} - a_k^L) \nonumber \\
&= \sum_d^K t_d a_k^L - \sum_d^K t_d \mathbb{1}_{d=k} \nonumber \\
&= a_k^L \sum_d^K t_d - t_k \nonumber \\
&= a_k^L - t_k
\end{align*} \tag{3}
$$

其中:

$$
\mathbb{1}_{d=k} =
    \begin{cases}
        1  & \quad \text{if } d=k \\
        0  & \quad \text{otherwise }.
    \end{cases}
$$


再由$\frac{\partial z_{k}^L}{\partial W_{kj}^L} = a_j^{L-1}$代入得:

$$
\begin{align}
    \frac{\partial E}{\partial W_{kj}^L} &= \frac{\partial E}{\partial z_{k}^L} \frac{\partial z_{k}^L}{\partial W_{kj}^L}
    = (a_k^L - t_k) a_j^{L-1}
\end{align}
$$

偏袒b:
$$
\begin{align}
\frac{\partial E}{\partial b_{k}^L} &= \frac{\partial E}{\partial z_{k}^L} \frac{\partial z_{k}^L}{\partial b_{k}^L} = a_k^L - t_k
\end{align}
$$

其他层方法相似.
