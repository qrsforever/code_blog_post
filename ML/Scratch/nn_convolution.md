---

title: (原创)神经网络之卷积过程实例

date: 2019-07-19 15:33:06
tags: [Scratch, Python]
categories: [ML]

---

<!-- vim-markdown-toc GFM -->

* [Introduction](#introduction)
    * [Notation](#notation)
    * [Full Connected Network](#full-connected-network)
    * [Convolutional Neural Network](#convolutional-neural-network)
    * [Standard Neural Network and Convolution Nerual Network](#standard-neural-network-and-convolution-nerual-network)
* [Codes](#codes)
    * [demo1](#demo1)
    * [demo2](#demo2)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Introduction

## Notation

symbols definitions used in full text is [here](https://qrsforever.github.io/2019/06/25/Tools/Math/symbols/#机器学习)

## Full Connected Network

for all errors(loss) function is:

$$
    \{E^{(n)}\} = \dfrac{1}{2} \sum^{N}_{n = 1} \sum^{c}_{k = 1} \big( t^{(n)}_k - y^{(n)}_k \big)^2
$$

for one item error of them is:

$$
    E^{(n)} = E = \dfrac{1}{2} \sum^{c}_{k = 1} \big( t^{(n)}_k - y^{(n)}_k \big)^2
                = \dfrac{1}{2} \begin{Vmatrix} \mathbf{t}^{(n)} - \mathbf{y}^{(n)} \end{Vmatrix}^2_2
                \label{error} \tag{1}
$$

at times, we simplify the equation by emitting the superscript ${(n)}$, just below:

$E = \dfrac{1}{2} \begin{Vmatrix} \mathbf{t} - \mathbf{y} \end{Vmatrix}^2_2 \label{error_one} \tag{2}$


the last output layer $\mathbf{y}^{[L]}$ breif as:

$$
    \begin{align*}
        \mathbf{y} &= \mathbf{a}^{[L]} \\
            &= \mathbf{a}^{[L]}\color{Red}{\bigg(\mathbf{z}^{[L]}\bigg)} \\
            &= \mathbf{a}^{[L]}\color{Red}{\bigg(\mathbf{W}^{[L]}
               \mathbf{a}^{[L-1]}\color{Blue}{\big(\mathbf{z}^{[L-1]}\big)} + \mathbf{b}^{[L]}\bigg)} \\
            &= \mathbf{a}^{[L]}\color{Red}{\bigg(\mathbf{W}^{[L]}
               \mathbf{a}^{[L-1]}\color{Blue}{\big(\mathbf{W}^{[L-1]}
               \mathbf{a}^{[L-2]}\color{Green}{(\mathbf{z}^{[L-2]})} + \mathbf{b}^{[L-1]}\big)} + \mathbf{b}^{[L]}\bigg)} \\
            &= \mathbf{a}^{[L]}\color{Red}{\bigg(\mathbf{W}^{[L]}
               \mathbf{a}^{[L-1]}\color{Blue}{\big(\mathbf{W}^{[L-1]}
               \mathbf{a}^{[L-2]}\color{Green}{(\mathbf{W}^{[L-2]}
               \mathbf{a}^{[L-3]}\color{Black}{(\mathbf{z}^{[L-3]})} + \mathbf{b}^{[L-2]})} + \mathbf{b}^{[L-1]}\big)} + \mathbf{b}^{[L]}\bigg)} \\
            &= \cdots
    \end{align*} \label{y_equ} \tag{3}
$$

let's have a look the derivative $\delta^{[L]}$ of the error with respect to the neurons $z^{[L]}$ of the last output layer:

from the equation $\ref{error_one}$ :

$$
    \delta^{L} = \dfrac{\partial E}{\partial {z^{[L]}}}
        = (\mathbf{y}-\mathbf{t}) \odot \dfrac{\partial \mathbf{a}^{[L]}(z^{[L]})}{\partial {z^{[L]}}}
$$

pre layer, using the chain rule of derivative:

$$
    \delta^{L-1} = \dfrac{\partial E}{\partial {z^{[L-1]}}}
        = \dfrac{\partial E}{\partial {z^{[L]}}} \dfrac{\partial {z^{[L]}}}{\partial \mathbf{a}^{[L-1]}(z^{[L-1]})}
        \dfrac{\partial \mathbf{a}^{[L-1]}(z^{[L-1]})}{\partial {z^{[L-1]}}}
$$

let $a^l = \sigma^l(z^l)$, then(matrix multiplication):

$$
    \delta^l = (W^{l+1})^T \delta^{l+1} \sigma'(z^{l})
$$

## Convolutional Neural Network

pass

## Standard Neural Network and Convolution Nerual Network

The difference between them is the calculate, one is matrix multiplication and another is convolution,
apart from this, they are nearly the same.

**weights sharing**

below we display the convert:

```

matrix:

  +----------------------+             weights
  |                      |
  |  1       2       3   |          +-------------+           +--------------+
  |  =       =           |          |  1       2  |           |  23      33  |
  |                      |          |             |           |              |
  |  4       5       6   |     *    |             |     =     |              |
  |  =       =           |          |  3       4  |           |  53      63  |
  |                      |          +-------------+           +--------------+
  |  7       8       9   |
  |                      |          first rotate 180     mode = "valid"
 +----------------------+
          Image               Kernel or Convolution matrices or Mask
          3 X 3                           2 X 2


 ----------------------------------------------------------------------------------

vector:

                        * 4 (w_22)
                    1 --------------\           WX + b
                        * 3 (w_21)   \
                    2 --------------\ \
                                     \ \   4 + 6
                    3              +  ------------->  23:
                        * 2 (w_12)   / /   8 + 5
                    4 --------------/ /
                        * 1 (w_11)   /                33
weight sharing  <-- 5 --------------/
                        * 4 (w_22)  \                 53
                    6 -------------\ \
                        * 3 (w_21)  \ \   20 + 18
                    7              + -------------->  63
                        * 2 (w_12)  / /   16 + 9
                    8 -------------/ /
                        * 1 (w_11)  /
                    9 -------------/

                 inputs                              hiden
                  9 X 1                              4 X 1

 ----------------------------------------------------------------------------------

the forward feed and back propagation.

### zero-padding

benifits:

- make hight and width same with the previous layer

- keep more information at the border of one image

```

Feedforward in CNN is identical with convolution operation:

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Scratch/convolution-mlp-mapping.png)


# Codes

## demo1

install skimage: `sudo pip3 install scikit-image`, this demo1 using numpy implemets the simple convolution
neural network.

{% asset_jupyter python3 notebook/nn_convolution.ipynb %}

## demo2

download dataset [mldata](http://yann.lecun.com/exdb/mnist/) or
[baidu](https://pan.baidu.com/s/1gAFZ9gSf4pHJBt5W6_PgPQ "提取码: gxk4")

This demo have a variable **validation_data** which (validation set) is mostly used to look out for
overfitting on the trainning dataset when trainning. **what is the difference between validation set and
test set?**

@neggert said
    "The validation set is checked during training to monitor progress, and possibly for early stopping,
    but is never used for gradient descent."

I most recommend the post 
[Convolutional Neural Networks](https://mukulrathi.com/demystifying-deep-learning/convolutional-neural-network-from-scratch/)

{% asset_jupyter python3 notebook/nn_convolution2.ipynb %}

# References

#. http://www.songho.ca/dsp/convolution/convolution2d_example.html?source=post_page

#. https://grzegorzgwardys.wordpress.com/2016/04/22/8/

#. http://neuralnetworksanddeeplearning.com/chap6.html

#. https://www.analyticsvidhya.com/blog/2018/12/guide-convolutional-neural-network-cnn/

#. https://becominghuman.ai/only-numpy-implementing-convolutional-neural-network-using-numpy-deriving-forward-feed-and-back-458a5250d6e4

#. https://www.kdnuggets.com/2018/04/building-convolutional-neural-network-numpy-scratch.html

#. https://codereview.stackexchange.com/questions/133251/a-cnn-in-python-without-frameworks

#. https://datascience-enthusiast.com/DL/Convolution_model_Step_by_Stepv2.html

#. [Notes on Convolutional Neural Networks](http://cogprints.org/5869/1/cnn_tutorial.pdf "recommended pdf")

#. [How the backpropagation algorithm works](http://neuralnetworksanddeeplearning.com/chap2.html "recommended")

#. https://mukulrathi.com/demystifying-deep-learning/conv-net-backpropagation-maths-intuition-derivation/

#. https://mukulrathi.com/demystifying-deep-learning/convolutional-neural-network-from-scratch/

#. https://qrsforever.github.io/2019/05/30/ML/Guide/activation_functions

#. https://qrsforever.github.io/2019/07/23/ML/Guide/conv_mode

#. https://pdfs.semanticscholar.org/5d79/11c93ddcb34cac088d99bd0cae9124e5dcd1.pdf
