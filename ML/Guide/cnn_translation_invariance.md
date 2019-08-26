---

title: (draft)ConvNet平移不变性

date: 2019-08-26 13:46:56
tags: [Guide, Draft]
categories: [ML]

---

<!-- vim-markdown-toc GFM -->

* [Introduction](#introduction)
* [Translation Picture](#translation-picture)
* [Invariance vs. Equivariance](#invariance-vs-equivariance)
* [Why and how](#why-and-how)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

不理解的地方比较多, 后续整理.

# Introduction

**Invariance** means that you can recognize an object as an object, even when its appearance varies in some way.

**Translation** means that each point/pixel in the image has been moved the same amount in the same
direction. Alternately, you can think of the origin as having been shifted an equal amount in the opposite
direction.

# Translation Picture

![](https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/ML/Guide/cnn_translation_invariance_1.png){.center}

# Invariance vs. Equivariance

Translation invariance means that the system produces exactly the same response, regardless of how its input is shifted.

Equivariance means that the system works equally well across positions, but its response shifts with the position of the target.

# Why and how

{% blockquote "Brando Miranda" https://www.quora.com/Why-and-how-are-convolutional-neural-networks-translation-invariant "Why and how are convolutional neural networks translation-invariant?" %}

Convolution + Max pooling $\approx$translation invariance

- Convolution: provides equivariance to translation.

- Pooling: provides the real translation invariance but only approximately.

{% endblockquote %}

{% blockquote "Aditya Kumar Praharaj" https://www.quora.com/Why-and-how-are-convolutional-neural-networks-translation-invariant "Why and how convolutional neural networks are translation-invariant?" %}

All this happens because of weight sharing (visualize the kernels as weight matrices; certain submatrices
of the weight matrix share the weights) in Convolutional Nets, which inherently allow this invariance.

{% endblockquote %}

{% blockquote  "Jean Da Rolt" https://www.quora.com/How-is-a-convolutional-neural-network-able-to-learn-invariant-features "How is a convolutional neural network able to learn invariant features?" %}

After some thought, I do not believe that the pooling operation is the main reason for the translation
invariant property in CNNs. I believe that invariance (at least to translation) is due to the convolution
filters (not specifically the pooling) while the fully-connected layers at the end are “position-dependent”.

{% endblockquote %}

# References

#. <https://www.quora.com/How-is-a-convolutional-neural-network-able-to-learn-invariant-features>
#. <http://cs231n.github.io/convolutional-networks/>
#. <https://stats.stackexchange.com/questions/208936/what-is-translation-invariance-in-computer-vision-and-convolutional-neural-netwo>
#. <https://www.quora.com/Why-and-how-are-convolutional-neural-networks-translation-invariant>
