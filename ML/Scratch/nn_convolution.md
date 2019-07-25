---

title: (原创)神经网络之卷积过程实例

date: 2019-07-19 15:33:06
tags: [Scratch, Python]
categories: [ML]

---

<!-- vim-markdown-toc GFM -->

* [Standard Neural Network and Convolution Nerual Network](#standard-neural-network-and-convolution-nerual-network)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Standard Neural Network and Convolution Nerual Network

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


                    1 --------------\           WX + b
                           * 4       \
                    2 --------------\ \
                           * 3       \ \   4 + 6
                    3              +  ------------->  23
                           * 2       / /   8 + 5
                    4 --------------/ /
                           * 1       /                33
weight sharing  <-- 5 --------------/
                           * 4      \                 53
                    6 -------------\ \
                           * 3      \ \   20 + 18
                    7              + -------------->  63
                           * 2      / /   16 + 9
                    8 -------------/ /
                           * 1      /
                    9 -------------/

                 inputs                              hiden
                  9 X 1                              4 X 1

 ----------------------------------------------------------------------------------

the forward feed and back propagation.

```




# References

#. http://www.songho.ca/dsp/convolution/convolution2d_example.html?source=post_page

#. https://grzegorzgwardys.wordpress.com/2016/04/22/8/

#. https://www.analyticsvidhya.com/blog/2018/12/guide-convolutional-neural-network-cnn/

#. https://becominghuman.ai/only-numpy-implementing-convolutional-neural-network-using-numpy-deriving-forward-feed-and-back-458a5250d6e4

#. https://www.kdnuggets.com/2018/04/building-convolutional-neural-network-numpy-scratch.html

#. https://codereview.stackexchange.com/questions/133251/a-cnn-in-python-without-frameworks

#. https://datascience-enthusiast.com/DL/Convolution_model_Step_by_Stepv2.html

#. https://qrsforever.github.io/2019/05/30/ML/Guide/activation_functions

#. https://qrsforever.github.io/2019/07/23/ML/Guide/conv_mode
