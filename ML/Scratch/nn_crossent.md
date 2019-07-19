---

title: (原创)神经网络之交叉熵代码实例

date: 2019-07-01 22:47:37
tags: [Scratch, Python]
categories: [ML]

---


<!-- vim-markdown-toc GFM -->

* [Intro](#intro)
* [SE and CE](#se-and-ce)
* [Gradient](#gradient)
* [Hiden](#hiden)
* [Codes](#codes)

<!-- vim-markdown-toc -->

<!-- more -->

```
    ____________________________________________________
   /\                                                   \
   \_|                                                  |
     | z_0^h = sum_i^I(w[i, 0]^h * inputNode_i + b_0^h) |
     |                                                  |
     | hideNode_0 = tanh(z_0^h)                         |
     |                                                  |
     |   _______________________________________________|_
      \_/_________________________________________________/
                                  |
                                  |   __________________________________________________
                                  |  /\                                                 \
                                  |  \_|                                                |
                                  |    | z_0^o = sum_j^J(w[j, 0]^o * hideNode_j + b_0^o |
                                  |    |                                                |
inputNode:  I                     |    | ouputNode = softmax(z_0^o)                     |
hidenNode:  J                     |    |                                                |
outputNode: K                     |    |   _____________________________________________|_
                                  |     \_/_______________________________________________/
                                  |                              |
                                  |                              |
                                  |                              |
                                  |                              |
                                  |                              |
                                  |                              |
        I                      J  |                     K        |
                                  |                              |
                                tanh(z_0^h)                softmax(z_0^o)
      *****                  *****                    *****
    **     **  w[0, 0]     **     **   w[0, 0]      **     **
    *       * -----------> *  b_0  * -------------> *       *
    **     **            > **     **              > **     **
      *****             / ^  *****              -/ ^  *****
                      -/ /                    -/  /
                     /   |                  -/   /
             w[1,0] /   /           w[1, 0]/    /
                  -/   /                 -/    /
                 /     |               -/      |
      *****     /     /     *****    -/       /
    **     ** -/     /    **     ** /        /
    *       *        |    *  b_1  *         /
    **     **       /     **     **        /
      *****        /        *****         /
        .         /           .          /
        .         |           .         /
        .        /            .        /
                /                      |
          w[i,0]|             w[j, 0] /
                /                    /
      *****    /            *****   /                 *****
    **     **_|           **     **-                **     **
    *       *             *  b_j  *                 *       *
    **     **             **     **                 **     **
      *****                 *****                     *****

   ith inputNode          jth hideNode            kth outputNode

```

# Intro

Above drawit, only one hiden layer with activation funciton **tanh**, and the output layer with activation
function **softmax**(归一化指数函数).

The input layer: have I nodes, the ith node is $x_i$.

The hiden layer: have J nodes, weight: $\theta_{ij}$, activation function: $tanh^{hiden}(z_j)$, shortening: $a^h(z_j)$
and $z_j = \sum_i^I \theta_{ij} x_i + b_i \label{z_j} \tag{1}$.

The output layer: have K nodes, weight: $\theta_{jk}$, activation function: $softmax^{output}(z_k)$, shortening: $a^o(z_k)$.
and $z_k = \sum_j^J \theta_{jk} x_j + b_j \label{z_k} \tag{2}$

$tanh(z_i)$ and its derivative (equation only contain $z_i$):

let $tanh(z_i) = \dfrac{1-e^{-2z_i}}{1 + e^{-2z_i}}$ then:

$$
\begin{align*}
tanh'(z_i) &= 1 - tanh^2(z_i) \\
         &= (1 + tanh(z_i))(1 - tanh(z_i)) \\
\end{align*}
$$

$softmax(z_i)$ and its partial derivative (equation not only contain $z_i$ but also contain other z-nodes in denominator):

let $softmax(z_i) = \dfrac{e^z_i}{\sum_k^K e^{z_k}}$ then:

$$
\left\{\begin{align*}
\text{ if } i = j; \ \ \dfrac{\partial softmax'(z_i)}{\partial z_i} &= \dfrac{e^{z_i} \sum_k^K e^{zk} - e^{z_i}e^{z_i}}{\left(\sum_k^K e^{z_k}\right)^2} \\
&= softmax(z_i)(1 - softmax(z_i)) \\
&= a_i(1-a_i) \label{i_eq_j}  \tag{3} \\ \\
\text{ if } i \neq j; \ \  \dfrac{\partial softmax'(z_j)}{\partial z_i} &= \dfrac{0 - e^{z_j} e^{z_i}}{\left(\sum_k^K e^{z_k}\right)^2} \\
&= -softmax(z_i)softmax(z_j) \\
&= -a_ia_j  \label{i_neq_j} \tag{4}
\end{align*}\right.
$$

# SE and CE

SE loss function:

$$
Loss = \sum_{k=1}^{K} \dfrac{1}{2} \left(a^{o}(z_k) - y_k\right)^2
$$

MSE(mean squared error): $\dfrac{Loss}{K}$

with $\ref{i_eq_j}$ and $\ref{i_neq_j}$, so the partial derivative of $z_k$ (the kth output node):

$$
\begin{align*}
\dfrac{\partial Loss}{\partial z_i} &= \dfrac{\partial \sum_{k=1}^{K} \dfrac{1}{2} \left( a^{o}(z_k) - y_k\right )^2}{\partial z_i}  \\
&= \sum_{k=1}^K \left(a^o(z_k) - y_k \right) \dfrac{\partial a^o(z_k)}{\partial z_i} \\ 
&= \sum_{k \neq i}^K \left(a^o(z_i) - y_i \right) \dfrac{\partial a^o(z_k)}{\partial z_i}
 + \sum_{k=i}^K \left(a^o(z_i) - y_i \right) \dfrac{\partial a^o(z_i)}{\partial z_i} \\
&= \sum_{k \neq i}^K \left(a^o(z_k) - y_k \right) \left( -a^o(z_k)a^o(z_i) \right ) 
 + \left(a^o(z_i) - y_i \right )a^o(z_i) \left( 1-a^o(z_i) \right ) \\
&\approx \left(a^o(z_i) - y_i \right )a^o(z_i) \left( 1-a^o(z_i) \right ) \label{se_z_k} \tag{5}
\end{align*}
$$

above, the last equation
$\ref{se_z_k}$ is because the softmax is 'one-hot' encoding, $\text{ if } k \neq i, \text{ then } a^o(z_k) a^o(z_i) \approx 0$

-----------------------------------------------------------------

CE loss function

$$
Loss = -\sum_{k=1}^K H\big(y_k, a^o(z_k)\big) = -\sum_{k=1}^K y_k log a^o(z_k)
$$

MCEE(mean crossentropy error): $\dfrac{Loss}{K}$

so the partial derivative of $z_k$, for short: Loss to L, $a^o(z_i)$ to $a_i$:

$$
\begin{align*}
\dfrac{\partial L}{\partial z_i} &= \sum_k^K \big(\frac{\partial L}{\partial a_k} \frac{\partial a_k}{\partial z_i}\big) \\
 &= \sum_{k\neq i}^K \big(\frac{\partial L}{\partial a_k} \frac{\partial a_k}{\partial z_i}\big)
  + \sum_{k=i}^K \big(\frac{\partial L}{\partial a_k} \frac{\partial a_k}{\partial z_i}\big) \\ 
 &= \sum_{k\neq i}^K \big( -y_k\frac{1}{a_k} \frac{\partial a_k}{\partial z_i} \big)
  + \big( -y_k\frac{1}{a_k} \frac{\partial a_k}{\partial z_i} \big) \\
 &= \sum_{k\neq i}^K \big( -y_k\frac{1}{a_k} (-a_i a_k) + \big( -y_i\frac{1}{a_i} a_i(1-a_i) \big) \\
 &= \sum_{k\neq i}^K a_iy_k + (-y_i(1-a_i)) \\
 &= \sum_{k\neq i}^K a_iy_k + a_iy_i -y_i \\
 &= a_i\sum_k^K y_k - y_i \\
 &= a_i - y_i \label{ce_z_k} \tag{6}
\end{align*}
$$

we call $\frac{\partial L}{\partial z_i}$ above is the middle calculate **signal**, the last output layer
as **oSignal**, the hiden layer as **hSignal**, the signal equation is different between SE and CE.

# Gradient

look back $\ref{z_j}$ and $\ref{z_k}$.

below is the weight **hoGrad** as the hiden node to output node just is the partial derivative of Loss to $\theta$,
the $hoGrad_{jk}$ denote the partial derivative of Loss to the weight where from the jth node in hiden layer and target to the ith output node:

$$
\text{hoGrad}_{jk} = \dfrac{\partial L}{\partial \theta_{jk}} = 
\dfrac{\partial L}{\partial z_k} \dfrac{\partial z_k}{\partial \theta_{jk}} =
\dfrac{\partial L}{\partial z_k} a^h(z_j) = \text{oSignal}_k\ a^h(z_j)
$$

and the bias **obGrad**: $\text{obGrad}_k = \dfrac{\partial L}{\partial b_{jk}} = \text{oSignal}_k\ * 1$

# Hiden

let $a_k$ short for $a^o(z_k)$ 

let $a_j$ short for $a^h(z_j)$

let **oSignal** short for list $\dfrac{\partial Loss}{\partial z_k}; k \in [1, K]$

let **hSignal** short for list $\dfrac{\partial Loss}{\partial z_j}; j \in [1, J]$

if $z_k = \theta_{jk} a_j + b_k;\ a_j = tanh(z_j);\ z_j = \theta_{ij} x_i + b_j$, then:
$$
\left\{\begin{matrix}
\dfrac{\partial z_k}{\partial \theta_{jk}} = a_j; & \dfrac{\partial z_k}{\partial b_k} = 1 \\
\dfrac{\partial z_j}{\partial \theta_{ij}} = x_i; & \dfrac{\partial z_j}{\partial b_j} = 1 \\
\end{matrix}\right. \label{d_weight_bias} \tag{7}
$$

and 

$$
\begin{matrix}
\dfrac{\partial z_k}{\partial a_j} = \theta_{jk}; & \dfrac{\partial z_j}{\partial x_i} = \theta_{ij} \\
\end{matrix} \label{d_a_x} \tag{8}
$$

and

$$
\begin{align*}
\dfrac{\partial a_j}{\partial z_j} &= \dfrac{\partial tanh(z_j)}{\partial z_j} \\
&= \big(1+tanh(z_j)\big)\big(1-tanh(z_j)\big) \\
&= (1+a_j)(1-a_j)
\end{align*} \label{d_tanh} \tag{9}
$$

from $\ref{d_weight_bias}$, $\ref{d_a_x}$ and $\ref{d_tanh}$, we can get
the partial derivative of the jth hide node $z_j$ by Loss is (each output node contain the weight of $z_j$):

$$
\begin{align*}
\text{hSignal}_j = \dfrac{\partial Loss}{\partial z_j} &= \sum_{k=1}^{K} \dfrac{\partial Loss}{\partial a_k}
    \dfrac{\partial a_k}{\partial z_k} \dfrac{\partial z_k}{\partial a_j} \dfrac{\partial a_j}{\partial z_j}  \\
 &= \sum_{k=1}^{K} \dfrac{\partial Loss}{\partial z_k} 
    \dfrac{\partial z_k}{\partial a_j} \dfrac{\partial a_j}{\partial z_j} \\
 &= \sum_{k=1}^{K} \text{oSignal}_k \dfrac{\partial z_k}{\partial a_j} \dfrac{\partial a_j}{\partial z_j} \\
 &= \sum_{k=1}^{K} \text{oSignal}_k \theta_{jk} \dfrac{\partial a_j}{\partial z_j} \\
 &= \dfrac{\partial a_j}{\partial z_j} \sum_{k=1}^{K} \text{oSignal}_k \theta_{jk} \\
 &= (1+a_j)(1-a_j) \sum_{k=1}^{K} \text{oSignal}_k \theta_{jk} \label{hsignal_z_j} \tag{10} \\
\end{align*}
$$

-----------------------------------------------------------------

for the partial derivative of the jth hide node's weight:

$$
\begin{align*}
\text{ihGrad}_{ij} &= \dfrac{\partial Loss}{\partial \theta_{ij}} \\
 &= \dfrac{\partial Loss}{\partial z_j} \dfrac{\partial z_j}{\partial \theta_{ij}} \\
 &= \text{hSignal}_j * x_i
\end{align*}
$$

# Codes

{% asset_jupyter python3 notebook/nn_crossent.ipynb %}

