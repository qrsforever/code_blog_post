---

title: (draft)梯度下降算法

date: 2019-09-20 10:18:25
tags: [Guide, Draft]
categories: [ML]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/ML/Guide/optimizing_gradient_descent.md)

<!-- vim-markdown-toc GFM -->

* [Keys](#keys)
* [Defination](#defination)
* [Draft](#draft)
* [Gradient descent](#gradient-descent)
    * [BGD (batch gradient descent)](#bgd-batch-gradient-descent)
    * [SGD (stochastic gradient descent)](#sgd-stochastic-gradient-descent)
    * [MGD (mini-batch gradient descent)](#mgd-mini-batch-gradient-descent)
* [Algorithms](#algorithms)
    * [Momentum](#momentum)
    * [NAG (Nesterov's accelerated gradient)](#nag-nesterovs-accelerated-gradient)
    * [Adagrad](#adagrad)
    * [Adadelta](#adadelta)
    * [RMSprop](#rmsprop)
    * [Adam](#adam)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Keys

损坏函数(误差函数), 凸误差函数, 非凸误差函数, 学习率, 鞍点, 参数更新

# Defination

w.r.t. : with regard to

{% blockquote ruder.io http://ruder.io/optimizing-gradient-descent An overview of gradient descent optimization algorithms %}

Gradient descent is a way to minimize an objective function $J(\theta)$ parameterized by a model's
parameters $\theta \in \mathbb R^d$ by updating the parameters in the opposite direction of the gradient
of the objective function $\nabla_\theta J(\theta)$ w.r.t. to the parameters. The learning rate $\eta$
determines the size of the steps we take to reach a (local) minimum.

{% endblockquote %}

# Draft

优化计算目标函数的梯度需要根据使用的数据的量的多少进行在`精度`和`时间`上权衡去择取合适的算法.

对于凸误差函数,批梯度下降法能够保证收敛到全局最小值,对于非凸函数,则收敛到一个局部最小值.

# Gradient descent

[代码实现(old)](https://qrsforever.github.io/2019/05/28/ML/Scratch/GD/)

## BGD (batch gradient descent)

批量梯度下降

*Defination*:

Vanilla gradient descent computes the gradient of the cost function w.r.t. to the parameters $\theta$ for
the entire training dataset:
$$
\theta = \theta - \eta \nabla_\theta J(\theta)
$$

**对全部训练数据进行依次更新**, 如果数据集比较大, 造成冗余计算.

*Code*:

> ```{.python .numberLines startFrom="1"}
> for i in range(nb_epochs):
>     params_grad = evaluate_gradient(loss_function, data, params)
>     params = params - learning_rate * params_grad
> ```

## SGD (stochastic gradient descent)

随机梯度下降法, 有时也叫` on-line `gradient descent, 对每一条训练数据进行一次更新.

*Defination*:

Stochastic gradient descent (SGD) performs a parameter update for each training example $x^{(i)}$ and
label $y^{(i)}$:
$$
\theta = \theta - \eta \nabla_\theta J(\theta; x^{(i)}; y^{(i)})
$$

**对每条训练数据进行一次更新**, 更新频繁会出现$J$值的上下波动, 反而可能跳到新的,潜在的更好的局部最优
上.

*Codes*:

> ```{.python .numberLines startFrom="1"}
> for i in range(nb_epochs):
>     np.random.shuffle(data)
>     for example in data:
>         params_grad = evaluate_gradient(loss_function, example, params)
>         params = params - learning_rate * params_grad
> ```

## MGD (mini-batch gradient descent)

小批量梯度下降法

*Defination*:

Mini-batch gradient descent finally takes the best of both worlds and performs an update for every
mini-batch of n training examples:
$$
\theta = \theta - \eta \nabla_\theta J(\theta; x^{(i, i+n)}; y^{(i, i+n)})
$$

*Codes*:

> ```{.python .numberLines startFrom="1"}
> for i in range(nb_epochs):
>     np.random.shuffle(data)
>     for batch in get_batches(data, batch_size=50):
>         params_grad = evaluate_gradient(loss_function, batch, params)
>         params = params - learning_rate * params_grad
> ```

**对一组训练数据进行一次更新**, 解决了SGD的跳动带来的收敛不稳定的问题, 有时机器学习中SGD可能就是指的是MGD.

# Algorithms

## Momentum

动量法

*Defination*:

Momentum is a method that helps accelerate SGD in the relevant direction and dampens oscillations. It does
this by adding a fraction $\gamma$ of the update vector of the past time step to the current update
vector:

$$
\begin{align}
    \begin{split}
    v_t &= \gamma v_{t-1} + \eta \nabla_\theta J( \theta) \\
    \theta &= \theta - v_t
    \end{split}
\end{align}
$$

SGD波动最容易发生在局部最优处, 假设从山顶到山底通过一个斜坡滑下(最斜的一条), 则动量会累加, 即越靠近山底,
滑下的速度越快, SGD因为抖动做不到, 而**动量法**的原理就是在从山顶到山底滑下的通道上(梯度向量)加上一个累加
分量$\gamma$, 如果滑下的过程中斜坡不变, $\gamma$一直累加, 即参数更新的step变大, 收敛更快.

{% blockquote ruder.io http://ruder.io/optimizing-gradient-descent/index.html#momentum momentum %}
The momentum term increases for dimensions whose gradients point in the same directions and reduces
updates for dimensions whose gradients change directions. As a result, we gain faster convergence and
reduced oscillation.
{% endblockquote %}

*Codes*:

> ```{.python .numberLines startFrom="1"}
> for i in range(nb_epochs):
>     np.random.shuffle(data)
>     for batch in get_batches(data, batch_size=50):
>         params_grad = evaluate_gradient(loss_function, batch, params)
>         v[:] = momentum * v + learning_rate * params_grad
>         params = params - v
> ```

## NAG (Nesterov's accelerated gradient)

内斯特罗夫加速梯度(Look ahead)

*Defination*:

Nesterov accelerated gradient (NAG) is a way to give our momentum term this kind of prescience, so we look
**ahead** by calculating the gradient not w.r.t. to our **current** parameters $\theta$ but w.r.t. the
approximate **future** position of our parameters:
$$
\begin{align}
    \begin{split}
    v_t &= \gamma v_{t-1} + \eta \nabla_\theta J( \theta - \gamma v_{t-1} ) \\
    \theta &= \theta - v_t
    \end{split}
\end{align}
$$

理解NAG, 关键要理解**超前预知**, 比如, 沿着斜坡下滑, 根据当前的下滑加速度(momentum), 提前预知下一时刻会到
达何处, 如果下一时刻到达的地方的斜率和当前不一样(eg.相反), 及时调整了加速度.

收敛速度比动量更新方法更快,收敛曲线更加稳定.

*Momentum vs NAG*:

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/Momentum_vs_NAG.png)

图片来自[^1]

[^1]: https://mc.ai/learning-parameters-part-2-momentum-based-and-nesterov-accelerated-gradient-descent

## Adagrad

自适应学习率调整

*Defination*:

Adagrad adapts the learning rate to the parameters, performing smaller updates (i.e. low learning rates)
for parameters associated with **frequently** occurring features, and larger updates (i.e. high learning
rates) for parameters associated with **infrequent** features.

单个参数更新形式1:
$$
\begin{align*}
g_{t, i} &= \nabla_\theta J( \theta_{t, i} ) \\
\theta_{t+1, i} &= \theta_{t, i} - \eta \cdot g_{t, i} \\
\theta_{t+1, i} &= \theta_{t, i} - \dfrac{\eta}{\sqrt{G_{t, ii} + \epsilon}} \cdot g_{t, i} \\
\end{align*}
$$

$G_{t} \in \mathbb{R}^{d \times d}$ is a diagonal matrix.

矩阵向量更新形式2:
$$
\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{G_{t} + \epsilon}} \odot g_{t}
$$

这个算法存在一个问题, $G_{t}$记载所有参数历史梯度累加平方和, 在整个训练过程中, 这个累加和不断增大, 这会导
致学习率变小, 无限变小时, 这个算法就会再也获取不到额外的信息.

## Adadelta

自适应学习率调整

*Defination*:

Adadelta is an extension of Adagrad, Instead of accumulating all past squared gradients, Adadelta
restricts the window of accumulated past gradients to some fixed size $w$

## RMSprop


## Adam


## AdaMax


## Nadam


## AMSGrad

# Compare

| 算法 | 优点 | 缺点 | 适用情况 |
|:-:|:-- |:-- |:--- |
| BGD | 目标函数为凸函数时,可以找到全局最优值 | 收敛速度慢,需要用到全部数据,内存消耗大 | 不适用于大数据集,不能在线更新模型 |
| SGD | 避免冗余数据的干扰,收敛速度加快,能够在线学习 | 更新值的方差较大,收敛过程会产生波动,可能落入极小值(卡在鞍点),选择合适的学习率比较困难(需要不断减小学习率) | 适用于需要在线更新的模型,适用于大规模训练样本情况 |
| Momentum | 能够在相关方向加速SGD,抑制振荡,从而加快收敛 | 需要人工设定学习率 | 适用于有可靠的初始化参数 |
| Adagrad | 实现学习率的自动更改 | 仍依赖于人工设置一个全局学习率,学习率设置过大,对梯度的调节太大.中后期,梯度接近于0,使得训练提前结束 | 需要快速收敛,训练复杂网络时;适合处理稀疏梯度1 |
| Adadelta | 不需要预设一个默认学习率,训练初中期,加速效果不错,很快,可以避免参数更新时两边单位不统一的问题 | 在局部最小值附近震荡,可能不收敛 | 需要快速收敛,训练复杂网络时 |
| Adam | 速度快,对内存需求较小,为不同的参数计算不同的自适应学习率 | 在局部最小值附近震荡,可能不收敛 | 需要快速收敛,训练复杂网络时;善于处理稀疏梯度和处理非平稳目标的优点,也适用于大多非凸优化 - 适用于大数据集和高维空间 |

# References

- <http://ruder.io/optimizing-gradient-descent/>
- <https://blog.csdn.net/google19890102/article/details/69942970>
- <https://blog.csdn.net/yzy_1996/article/details/84618536>
- <http://louistiao.me/notes/visualizing-and-animating-optimization-algorithms-with-matplotlib/>
- <https://towardsdatascience.com/10-gradient-descent-optimisation-algorithms-86989510b5e9>
- <https://colab.research.google.com/drive/1Xsv6KtSwG5wD9oErEZerd2DZao8wiC6h>
- <https://mc.ai/learning-parameters-part-2-momentum-based-and-nesterov-accelerated-gradient-descent/>
- <https://gist.github.com/akshaychandra21>
- <https://blog.csdn.net/yzy_1996/article/details/84618536>
