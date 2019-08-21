---

title: (draft)数据集介绍

date: 2019-08-21 10:03:49
tags: [Guide]
categories: [ML]

---

<!-- vim-markdown-toc GFM -->

* [CIFAR](#cifar)
    * [CIFAR10](#cifar10)
    * [CIFAR100](#cifar100)
* [MNIST](#mnist)
* [KMNIST](#kmnist)
* [FashionMNIST](#fashionmnist)
* [EMNIST](#emnist)
    * [Balanced](#balanced)
    * [Digits](#digits)
    * [Letters](#letters)
    * [MNIST](#mnist-1)
* [Chars74k](#chars74k)
* [Dogs](#dogs)
* [Animals](#animals)
* [Fruits360](#fruits360)
* [Boats](#boats)
* [VOC](#voc)

<!-- vim-markdown-toc -->

<!-- more -->

# CIFAR

[INTRO](http://www.cs.toronto.edu/~kriz/cifar.html)

The CIFAR-10 and CIFAR-100 are labeled subsets of the 80 million tiny images dataset.

## CIFAR10

The CIFAR-10 dataset consists of 60000 32x32 colour images in 10 classes, with 6000 images per class.
There are 50000 training images and 10000 test images.

## CIFAR100

This dataset is just like the CIFAR-10, except it has 100 classes containing 600 images each. There are
500 training images and 100 testing images per class.

<div class="alert alert-info"> <p>

**dataset**: 10000 x 3072 = 10000(test images) x 32(width) x 32(height) x 3(channels:RGB)

**labels**: a list of 10000 numbers in the range 0-9, **labels_names** in the batches.meta files.

</p></div>

-----------------------------------------------------------------

# MNIST

[INTRO](http://yann.lecun.com/exdb/mnist)

The MNIST database of handwritten digits, available from this page, has a training set of 60,000 examples,
and a test set of 10,000 examples. The digits have been size-normalized and centered in a fixed-size image
of 28x28 pixels.

The MNIST database is a subset of a much larger datasetknown as the NIST Special Database 19 which
contains handwritten digits and characters collected from over 500 writers.

The MNIST database was constructed from NIST's Special Database 3 and Special Database 1 which contain
binary images of handwritten digits. NIST originally designated SD-3 as their training set and SD-1 as
their test set.

The MNIST training set is composed of 30,000 patterns from SD-3 and 30,000 patterns from SD-1. Our test
set was composed of 5,000 patterns from SD-3 and 5,000 patterns from SD-1. The 60,000 pattern training set
contained examples from approximately 250 writers.

<div class="alert alert-info"> <p>

**size**: 28(width) x 28(height)

In the original dataset each pixel of the image is represented by a value between 0 and 255, where 0 is
black, 255 is white and anything in between is a different shade of grey.

</p></div>

MNIST is often the first dataset researchers try, they said:

{% blockquote %}

" If it doesn't work on MNIST, it won't work at all, Well, if it does work on MNIST, it may still fail on
others."

{% endblockquote %}

-----------------------------------------------------------------

# KMNIST

[INTRO](https://www.simonwenkel.com/2018/12/18/Kuzushiji-MNIST.html)

The Kuzushiji-MNIST or KMNIST dataset contains 10 classes of hiragana(日语) characters with a resolution
of 28x28 (grayscale) similar to MNIST. In total it contains 70000 images, 60000 for training and 10000 for
testing.

-----------------------------------------------------------------

# FashionMNIST

[INTRO](http://www.worldlink.com.cn/en/osdir/fashion-mnist.html)

Fashion-MNIST is a dataset of Zalando's article images—consisting of a training set of 60,000 examples and
a test set of 10,000 examples. Each example is a 28x28 grayscale image, associated with a label from 10
classes. We intend Fashion-MNIST to serve as a direct drop-in replacement for the original MNIST dataset
for benchmarking machine learning algorithms. It shares the same image size and structure of training and
testing splits.

-----------------------------------------------------------------

# EMNIST

[INTRO](https://www.westernsydney.edu.au/bens/home/reproducible_research/emnist)

[arxiv](https://arxiv.org/pdf/1702.05373.pdf)

Extended Modified NIST (EMNIST).

Structure and Organization of the EMNIST datasets:


 Name   | Classes | No. Training | No. Testing | Validation | Total
:------:|:-------:|:------------:|:-----------:|:----------:|:-----:
ByClass |   62    |   697,932    |   116,323   |    No      | 814,255
ByMerge |   47    |   697,932    |   116,323   |    No      | 814,255
Balanced|   47    |   112,800    |   18,800    |    Yes     | 131,600
Digits  |   10    |   240,000    |   40,000    |    Yes     | 280,000
Letters |   37    |   88,800     |   14,800    |    Yes     | 103,600
MNIST   |   10    |   60,000     |   10,000    |    Yes     | 70,000


## Balanced

total 131600, 47 classes, 2400 train examples and 400 test examples for each classs

<div class="alert alert-info"> <p>

131600 = 47 x (2400 + 400)

</p></div>

## Digits

total 28000, 10 classes, 2400 train examples and 400 test examples for each classs

<div class="alert alert-info"> <p>

28000 = 10 x (2400 + 400)

</p></div>

## Letters

total 145600, 26 classes, 5600 train examples and 800 test examples for each classs

<div class="alert alert-info"> <p>

145600 = 26 x (5600 + 800)

</p></div>

## MNIST

total 70000, 10 classes, 6000 train examples and 1000 test examples for each classs

<div class="alert alert-info"> <p>

70000 = 10 x (6000 + 1000)

</p></div>

-----------------------------------------------------------------

# Chars74k

[INTRO](http://www.ee.surrey.ac.uk/CVSSP/demos/chars74k/)

Character recognition datasets which consisting of all English alphabet letters in uppercase as well as
lowercase , along with the digits 0-9

The Chars74k dataset consists of:

- 64 classes (0-9, A-Z, a-z)
- 7705 characters obtained from natural images
- 3410 hand drawn characters using a tablet PC
- 62992 synthesised characters from computer fonts

This gives a total of over 74K images.

<div class="alert alert-info"> <p>

TODO

</p></div>

-----------------------------------------------------------------

# Dogs

[INTRO](https://www.kaggle.com/jessicali9530/stanford-dogs-dataset)

The Stanford Dogs dataset contains images of 120 breeds of dogs from around the world. This dataset has
been built using images and annotation from ImageNet for the task of fine-grained image categorization.

Contents of this dataset:

- Number of categories: 120
- Number of images: 20,580
- Annotations: Class labels, Bounding boxes

<div class="alert alert-info"> <p>

TODO

</p></div>

-----------------------------------------------------------------

# Animals

<div class="alert alert-info"> <p>

TODO

</p></div>

-----------------------------------------------------------------

# Fruits360

[INTRO](https://github.com/Horea94/Fruit-Images-Dataset)

A dataset of images containing fruits.

<div class="alert alert-info"> <p>

TODO

</p></div>

-----------------------------------------------------------------

# Boats

<div class="alert alert-info"> <p>

TODO

</p></div>

-----------------------------------------------------------------

# VOC

TODO

-----------------------------------------------------------------

# ADE20K

[INTRO-Full](http://groups.csail.mit.edu/vision/datasets/ADE20K/) </br>
[INTRO-Scene](http://sceneparsing.csail.mit.edu/)

Contains more than 20K scene-centric images exhaustively annotated with objects and object parts.
Specifically, the benchmark is divided into 20K images for training, 2K images for validation, and another
batch of held-out images for testing.

It has 20210 training images and 2000 validation images.

## ADEChallengeData2016

[INTRO](http://sceneparsing.csail.mit.edu/index_challenge.html)

-----------------------------------------------------------------

# COCO

TODO

-----------------------------------------------------------------

# References

#. <http://www.manongjc.com/article/27377.html>

#. <https://github.com/chainer/chainercv/tree/master/chainercv/datasets>
