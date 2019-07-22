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

[百度云盘Dataset](https://pan.baidu.com/s/1gAFZ9gSf4pHJBt5W6_PgPQ "提取码: gxk4")

{% asset_jupyter python3 notebook/backpropagation_algorithm.ipynb %}


# References

1. `https://qrsforever.github.io/2019/05/30/ML/Guide/activation_functions/`

2. `https://machinelearningmastery.com/implement-backpropagation-algorithm-scratch-python/`

3. `https://www.jianshu.com/p/284581d9b189`
