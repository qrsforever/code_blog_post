---

title: 循环神经网络RNN介绍

date: 2019-09-12 13:52:48
tags: [Guide]
categories: [ML]

---

# 关键字

神经注意力模块(Attention) = 向前预测单元 + 后向回顾单元

# 长期依赖

LSTM 解决避免长时期依赖(long-term dependency)的问题

# 梯度消失

LSTM 在某种程度上可以克服梯度消失问题.

![传统后向传播](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/rnn_grad_vanishing.gif)

![时间后向传播](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/rnn_time_grad_vanishing.gif)

图片来自[^1]

[^1]: https://baijiahao.baidu.com/s?id=1612358810937334377&wfr=spider&for=p

# 应用场景

## 语音识别

## 语言翻译

## 股票预测

## 图像识别(图里的内容)


# References

- [Understanding LSTM Networks](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)
- [译 Understanding LSTM Networks](https://www.cnblogs.com/mfryf/p/7904017.html)
- [如何深度理解RNN?——看图就好!](https://baijiahao.baidu.com/s?id=1612358810937334377&wfr=spider&for=pc)
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
