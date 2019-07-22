---

title: (原创)神经网络之反向传播实例

date: 2019-07-19 21:41:28
tags: [Scratch, Python]
categories: [ML]

---

<!-- vim-markdown-toc GFM -->

* [Codes](#codes)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Description

Implement the backpropagation algorithm in python, but use third libs(numpy, pandas...)

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
                                              ***********   delta


```

# Codes

{% asset_jupyter python3 notebook/backpropagation_algorithm.ipynb %}


# References

1. `https://qrsforever.github.io/2019/05/30/ML/Guide/activation_functions/`

2. `https://machinelearningmastery.com/implement-backpropagation-algorithm-scratch-python/`

3. `https://www.jianshu.com/p/284581d9b189`
