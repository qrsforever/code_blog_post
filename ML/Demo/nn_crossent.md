---

title: 神经网络之交叉熵实例

date: 2019-07-01 22:47:37
tags: [Demo, Python]
categories: [ML]

---


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

<!-- more -->

## Intro

Above drawit, only one hiden layer with activation funciton **tanh**, and the output layer with activation
function **softmax**(归一化指数函数).

The input layer: have I nodes, the ith node is $x_i$.

The hiden layer: have J nodes, weight: $\theta_{ij}$, activation function: $tanh^{hiden}(z_j)$, shortening: $a^h(z_j)$.

The output layer: have K nodes, weight: $\theta_{jk}$, activation function: $softmax^{output}(z_k)$, shortening: $a^o(z_k)$.

tanh(z_i) and its derivative (equation only contain $z_i$):

$$
\begin{align*}
tanh(z_i) &= \dfrac{1-e^{-2z_i}}{1 + e^{-2z_i}} \\ \\ 
tanh'(z_i) &= 1 - tanh^2(z_i) \\
         &= (1 + tanh(z_i))(1 - tanh(z_i)) \\
\end{align*}
$$

softmax(z_i) and its partial derivative (equation not only contain $z_i$ but also contain other z-nodes in denominator):

$$
\begin{align*}
softmax(z_i) &= \dfrac{e^z_i}{\sum_k^K e^{zk}} \\ \\
j = i: \\
\dfrac{\partial softmax'(z_i)}{\partial z_i} &= \dfrac{e^{z_i} \sum_k^K e^{zk} - e^{z_i}e^{z_i}}{\left(\sum_k^K e^{zk}\right)^2} \\
& = softmax(z_i)(1 - softmax(z_i)) \\ \\
j \neq i: \\
\dfrac{\partial softmax'(z_j)}{\partial z_i} &= \dfrac{0 - e^{z_j} e^{z_i}}{\left(\sum_k^K e^{zk}\right)^2} \\
& = -softmax(z_i)softmax(z_j)
\end{align*}
$$

## SE and CE

SE loss function:

$$
Loss = \sum_{k=1}^{K} \dfrac{1}{2} \left(a^{o}(z_k) - y_k\right)^2
$$

the partial derivative of $z_k$ (the kth output node):

$$
\begin{align*}
\dfrac{\partial Loss}{\partial z_i} &= \dfrac{\partial \sum_{k=1}^{K} \dfrac{1}{2} \left(a^{o}(z_k) - y_k\right)^2}{\partial z_i} \\
& = \dfrac {\dfrac{1}{2} \left(a^{o}(z_1) - y_1\right)^2}{\partial z_i} +
    \dfrac {\dfrac{1}{2} \left(a^{o}(z_2) - y_2\right)^2}{\partial z_i} \cdots +
    \dfrac {\dfrac{1}{2} \left(a^{o}(z_K) - y_K\right)^2}{\partial z_i} \\
\end{align*}
$$


