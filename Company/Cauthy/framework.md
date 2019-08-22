---

title: (draft)机器学习CV框架

date: 2019-08-21 17:23:32
tags: [Cauchy]
categories: [Company]

---

<link href="/static/css/qrs.css" rel="stylesheet">

<!-- vim-markdown-toc GFM -->

* [Introduction](#introduction)
    * [nginx](#nginx)
    * [yii](#yii)
    * [Visdom](#visdom)
    * [torchcv](#torchcv)
* [Cauchy](#cauchy)
    * [Architecture](#architecture)
    * [Start Servers](#start-servers)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

# Introduction

## nginx

Nginx [engine x] is an HTTP and reverse proxy server, a mail proxy server, and a generic TCP/UDP proxy
server.

See more [HOME PAGE](http://nginx.org/en/)

## yii

Yii is a fast, secure, and efficient PHP framework.

The name Yii (pronounced Yee or [ji:]) means "simple and evolutionary" in Chinese. It can also be thought
of as an acronym for **Yes It Is!**

![](https://www.yiiframework.com/doc/guide/2.0/en/images/application-structure.png)

See more [HOME PAGE](https://www.yiiframework.com/), [GITHUB](https://github.com/yiisoft/yii2)

## Visdom

Visdom is a visualization tool that generates rich visualizations of live data to help researchers and
developers stay on top of their scientific experiments that are run on remote servers. Visualizations in
Visdom can be viewed in browsers and easily shared with others.

See more [HOME PAGE](https://ai.facebook.com/tools/visdom/),
[GITHUB](https://github.com/facebookresearch/visdom)

## torchcv

A PyTorch-Based Framework for Deep Learning in Computer Vision.

Implemented Papers

- [Image Classification](https://github.com/youansheng/torchcv/tree/master/methods/cls)
    - VGG: Very Deep Convolutional Networks for Large-Scale Image Recognition
    - ResNet: Deep Residual Learning for Image Recognition
    - DenseNet: Densely Connected Convolutional Networks
    - ShuffleNet: An Extremely Efficient Convolutional Neural Network for Mobile Devices
    - ShuffleNet V2: Practical Guidelines for Ecient CNN Architecture Design

- [Semantic Segmentation](https://github.com/youansheng/torchcv/tree/master/methods/seg)
    - DeepLabV3: Rethinking Atrous Convolution for Semantic Image Segmentation
    - PSPNet: Pyramid Scene Parsing Network
    - DenseASPP: DenseASPP for Semantic Segmentation in Street Scenes

- [Object Detection](https://github.com/youansheng/torchcv/tree/master/methods/det)
    - SSD: Single Shot MultiBox Detector
    - Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks
    - YOLOv3: An Incremental Improvement
    - FPN: Feature Pyramid Networks for Object Detection

- [Pose Estimation](https://github.com/youansheng/torchcv/tree/master/methods/pose)
    - CPM: Convolutional Pose Machines
    - OpenPose: Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields

- [Instance Segmentation](https://github.com/youansheng/torchcv/tree/master/methods/seg)
    - Mask R-CNN

- [Generative Adversarial Networks](https://github.com/youansheng/torchcv/tree/master/methods/gan)
    - Pix2pix: Image-to-Image Translation with Conditional Adversarial Nets
    - CycleGAN: Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks.


See more [GITHUB](https://github.com/donnyyou/torchcv)

-----------------------------------------------------------------

# Cauchy

## Architecture

```
                        http://117.51.150.168:80/index.php?r=cauchy/myproject

                                      +---------------+
                                      |               |
                    +-----------------|    Browser    |
                    |                 |     UI        |
                    |                 +---------------+
                    |  80                          ^
                    v                               \  visdom
              +-------------+                        \
              | Nginx Proxy |                         \  8141 ~ 8199
              +-------------+                          \
                    |                                   \                      +-------------------+
                    |  10081                             \                     |                   |
                    v                                     \                    |   weights host    |
      +------------------------------+   index.php         \                   |                   |
      |         Web (enter)          |------+               \                  |    Http Server    |
      +------------------------------|      |                \                 |                   |
      |         Yii PHP Core         |      |                 \                +-------------------+
      +------------------------------+      |                  \                            28889
      |         Controllers          |<-----+                   \
      |           ^     ^            |    r=cauchy/action        \
      |          /       \           |                            v
      |         v         v          |                   +------------------------+
      |      Views       Models      |                   |      | Visdom Process  |
      +------------------------------+                   |      +-----------------+
                            |                            |      |                 |
                            |    /cauchy/train   8399    |Flask |Cauchy Framework |
                            +------------------------->  |      |                 |
                               /cauchy/free_port         |      +-----------------+
                                                         |      | Task Process    |
                                                         +------+-----------------+
```

## Start Servers

1. start the **http server** for downloading the pretrained models weights, listen 28889.

> `python -m http.server --directory=/path/to/pretrained_models 28889`

2. start the **nginx proxy**, listen 80, forword 80 to 10081(Yii).

> `/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf`

3. start **yii server** for interaction with web page, and cauchy framework, listen 10081.

> `php /path/to/basic/yii serve $hostip --port 10081`

4. start **flask server** for listening the task from yii server, listen 8399.

> `cd /path/to/test/flask_services; python cauchy_services.py --port 8339`

## Dynamic Process

see `test/flask_services/cauchy_services.py`

1. start the **visdom process** when flask server received the *`/cauchy/free_port`* command.

> `python -m visdom.server -port 8140`

2. start the **task process** (train/test) when flask server received the *`/cauchy/train[test]`* command

> `python run_tasks.py --hypes /path/to/hypes.json --viz_pid 21995 --viz_port 8140 --tmp_dir /tmp/tmpcyj84t5t`

**页面上每启动一个训练或评估任务, 都会对应的启动这个两个进程, 所有他们可以存在多个**

## Project Directory

```
├── apis                   //
│   └── cocoapi
│       ├── common
│       ├── LuaAPI
│       ├── MatlabAPI
│       └── PythonAPI
├── cauchy                 // 框架核心
│   ├── datasets           // 数据集操作: \
│   │   ├── automl                1. 数据集格式成框架可以识别的结构(独立于框架) \
│   │   ├── cls                   2. 迭代数据集
│   │   ├── det
│   │   ├── ins
│   │   ├── pose
│   │   ├── seg
│   │   ├── tools
│   │   └── track
│   ├── extensions         //
│   │   ├── ops
│   │   └── tools
│   ├── methods            // 负责任务主逻辑, 如: 数据加载, 算法模型选择, 模型评估, 训练监控
│   │   ├── automl                1. 根据参数"task"进行任务类型的选择(cls, det..), 确定真正的runner执行体. \
│   │   ├── cls                   2. 根据参数"method"选择算法模型, 然后进行训练(forward/backword). \
│   │   ├── det
│   │   ├── pose
│   │   ├── seg
│   │   └── tools
│   ├── metrics            // 评估算法模型(针对val/test数据集)
│   │   ├── cls
│   │   ├── det
│   │   ├── pose
│   │   └── seg
│   ├── models             // 根据以有的papers实现好的神经网络模型以及提供了损失方法
│   │   ├── automl
│   │   ├── backbones
│   │   ├── cls
│   │   ├── det
│   │   ├── pose
│   │   ├── rcnn
│   │   ├── seg
│   │   └── tools
│   └── utils              //
│       ├── helpers
│       ├── parser
│       ├── tools
│       └── visualizer
├── scripts                //
│   ├── cls
│   │   └── cifar
│   ├── det
│   │   └── voc
│   └── seg
│       ├── ade20k
│       └── cityscapes
└── test
    ├── cauchy             // 单独调试算法模型, 不依赖WEB控制及传参
    │   ├── hypes
    │   └── proj1
    └── flask_services     // Flask服务启动脚本, 与框架交互的唯一入口
        └── project
```

# References

- <https://www.cnblogs.com/jerehedu/p/7762046.html>
- <https://blog.csdn.net/fujian9544/article/details/79567090>
