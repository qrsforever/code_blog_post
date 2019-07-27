---

title: (原创)神经网络之反向传播实例

date: 2019-07-19 21:41:28
tags: [Scratch, Python]
categories: [ML]

---

<!-- vim-markdown-toc GFM -->

* [Description](#description)
* [Drawit](#drawit)
* [Codes](#codes)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Description

Implement the backpropagation algorithm in python, but not use third libs(numpy, pandas...)

Demo is a standard neural network structure: one input layer, one hiden layer and one output layer.

The activite function of the hiden and ouput nodes is sigmoid.

The evaluation of the backpropagation algorithm use cross validation K-folds which make good use for limited dataset.

# Algorithm

The backpropagation equations provide us with a way of computing the gradient of the cost function. Let's explicitly write this out in the form of an algorithm:

1. Input x: Set the corresponding activation $a^1$ for the input layer.
2. Feedforward: For each $l = 2, 3, \ldots, L$ compute $z^l=w^la^{l−1}+b^l and a^l=\sigma(z^l)$.
3. Output error $\delta^L$: Compute the vector $\nabla_a C \odot \sigma'(z^L)$.
4. Backpropagate the error: For each $l = L-1, L-2, \ldots, 2$ compute $\delta^{l} = ((w^{l+1})^T \delta^{l+1}) \odot \sigma'(z^{l})$.
5. Output: The gradient of the cost function is given by $\frac{\partial C}{\partial w^l_{jk}} = a^{l-1}_k \delta^l_j$ and $\frac{\partial C}{\partial b^l_j} = \delta^l_j$.


# Drawit

```


                             neuron
                           ***********
                        ***           ***
                      **       output    **
                     *                     *
                     *   sigmoid(WX + b)   *
                     *                     *
                      **      weights    **
                        ***           ***
                           ***********   delta: the middle signal. the derivative of the final activite function


```

# Codes

[百度云盘Datasets](https://pan.baidu.com/s/1gAFZ9gSf4pHJBt5W6_PgPQ "提取码: gxk4")

{% asset_jupyter python3 notebook/backpropagation_algorithm.ipynb %}


# References

1. `https://qrsforever.github.io/2019/05/30/ML/Guide/activation_functions/`

2. `https://machinelearningmastery.com/implement-backpropagation-algorithm-scratch-python/`

3. `https://www.jianshu.com/p/284581d9b189`
