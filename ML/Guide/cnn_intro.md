---

title: 卷积神经网络CNN介绍

date: 2019-06-03 18:08:15
tags: [Guide]
categories: [ML]

---


## 网络资源

[Intuitively Understanding Convolutions for Deep Learning][1] </br>
[卷积神经网络CNN总结][2] </br>
[卷积要旋转180度][3] </br>
[卷积神经网络简介][4] </br>
[别怕,"卷积"其实很简单][5] </br>
[wikipedia:convolution][6] </br>
[wikipedia:卷积][7] </br>
[mathworld][8] </br>
[图形Demo1][9] </br>
[图形Demo2][10] </br>
[最容易理解的对卷积(convolution)的解释][11] </br>
[Understanding Convolutions][12] </br>
[离散卷积过程举例图示详解][13] </br>
[必看: ConvNets][14] </br>


[推荐: 卷积介绍][100] </br>

[1]:https://towardsdatascience.com/intuitively-understanding-convolutions-for-deep-learning-1f6f42faee1
[2]:https://www.cnblogs.com/skyfsm/p/6790245.html
[3]:https://blog.csdn.net/leadai/article/details/83353470
[4]:https://zhuanlan.zhihu.com/p/25249694
[5]:https://blog.csdn.net/qq_39521554/article/details/79083864
[6]:https://en.wikipedia.org/wiki/Convolution
[7]:https://zh.wikipedia.org/wiki/%E5%8D%B7%E7%A7%AF
[8]:http://mathworld.wolfram.com/Convolution.html
[9]:https://lpsa.swarthmore.edu/Convolution/CI.html
[10]:https://phiresky.github.io/convolution-demo/
[11]:https://blog.csdn.net/bitcarmanlee/article/details/54729807
[12]:https://colah.github.io/posts/2014-07-Understanding-Convolutions/
[13]:https://blog.csdn.net/heshiip/article/details/79223442
[14]:http://cs231n.github.io/convolutional-networks/

[100]:https://lpsa.swarthmore.edu/Convolution/Convolution.html

<!-- more -->

## 重要语句

卷积的重要的物理意义是:一个函数(如:单位响应)在另一个函数(如:输入信号)上的**加权叠加**.
对于线性时不变系统,如果知道该系统的单位响应,那么将单位响应和输入信号求卷积,就相当于把
输入信号的各个时间点的单位响应**加权叠加**,就直接得到了输出信号.系统的零状态响应等于单位
冲击响应卷积上输入函数.
