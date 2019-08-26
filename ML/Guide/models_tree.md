---

title: (draft)机器学习算法汇总

date: 2019-08-26 19:55:07
tags: [Guide, Draft]
categories: [ML]

---


<!-- vim-markdown-toc GFM -->

* [AlphaTree](#alphatree)
    * [Object Classification](#object-classification)
        * [LeNet](#lenet)
        * [AlexNet](#alexnet)
        * [VGG](#vgg)
    * [Object Detection](#object-detection)
    * [Object Segmentation](#object-segmentation)

<!-- vim-markdown-toc -->

<!-- more -->

# AlphaTree

很多论文对算法模型的描述都有自己的风格, 对于我们这些刚入门的小生算是一个大大的挑战, 而我们希望有个大牛能用
一种绘图描绘方式统一描述这些模型,  这个希望被[深度神经网络(DNN)与对抗神经网络(GAN)模型总览图示][AT1]实现了.
这个**AlphaTree**可贵之处是定义了一套图标, 如下: ![图标][AT2]

Conv + Max: ![缩写][AT3].

止于重复造轮子, 详细的算法模式可以直接进入[here][AT1].

**如果图上再标上所使用的激活函数就更美了**

[AT1]: https://github.com/weslynn/AlphaTree-graphic-deep-neural-network
[AT2]: https://raw.githubusercontent.com/weslynn/AlphaTree-graphic-deep-neural-network/master/modelpic/cellsreadme.png
[AT3]: https://raw.githubusercontent.com/weslynn/AlphaTree-graphic-deep-neural-network/master/modelpic/equal.png

-----------------------------------------------------------------

## Object Classification

![cls](https://github.com/weslynn/graphic-deep-neural-network/raw/master/map/ObjectClassification.png)

### LeNet

1. 论文["Gradient-based learning applied to document recognition"][LeNet1]

2. 模型结构(激活函数From Sigmoid to ReLU)

![][LeNet2]

3. 数据变化

![][LeNet3]

see [more][LeNet0] for details.

[LeNet0]: https://github.com/weslynn/AlphaTree-graphic-deep-neural-network/blob/master/object%20classification%20%E7%89%A9%E4%BD%93%E5%88%86%E7%B1%BB/LeNet.md
[LeNet1]: http://yann.lecun.com/exdb/publis/pdf/lecun-01a.pdf
[LeNet2]: https://raw.githubusercontent.com/weslynn/AlphaTree-graphic-deep-neural-network/master/modelpic/lenet.png
[LeNet3]: https://raw.githubusercontent.com/weslynn/AlphaTree-graphic-deep-neural-network/master/modelpic/lenet_data2.png

### AlexNet

1. 论文["Imagenet classification with deep convolutional neural networks"][AlexNet1]

2. 模型结构(移除LRN层, 激活函数ReLU) ![][AlexNet2]

3. 数据变化 ![][AlexNet3]

see [more][AlexNet0] for details.

参考代码:

```.python
class AlexNet(nn.Module):
    def __init__(self, num_classes=1000):
        super(AlexNet, self).__init__()
        self.features = nn.Sequential(
            nn.Conv2d(3, 64, kernel_size=11, stride=4, padding=2),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=3, stride=2),
            nn.Conv2d(64, 192, kernel_size=5, padding=2),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=3, stride=2),
            nn.Conv2d(192, 384, kernel_size=3, padding=1),
            nn.ReLU(inplace=True),
            nn.Conv2d(384, 256, kernel_size=3, padding=1),
            nn.ReLU(inplace=True),
            nn.Conv2d(256, 256, kernel_size=3, padding=1),
            nn.ReLU(inplace=True),
            nn.MaxPool2d(kernel_size=3, stride=2),
        )
        self.avgpool = nn.AdaptiveAvgPool2d((6, 6))
        self.classifier = nn.Sequential(
            nn.Dropout(),
            nn.Linear(256 * 6 * 6, 4096),
            nn.ReLU(inplace=True),
            nn.Dropout(),
            nn.Linear(4096, 4096),
            nn.ReLU(inplace=True),
            nn.Linear(4096, num_classes),
        )

    def forward(self, x):
        x = self.features(x)
        x = self.avgpool(x)
        x = x.view(x.size(0), 256 * 6 * 6)
        x = self.classifier(x)
        return x
```

<div class="alert alert-info">**注意:**<p>

Here the output channel is 64 not 96, and using zero-padding(2)

</p></div>

[AlexNet0]: https://github.com/weslynn/AlphaTree-graphic-deep-neural-network/blob/master/object%20classification%20%E7%89%A9%E4%BD%93%E5%88%86%E7%B1%BB/AlexNet.md
[AlexNet1]: http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf
[AlexNet2]: https://raw.githubusercontent.com/weslynn/AlphaTree-graphic-deep-neural-network/master/modelpic/alexnettf.png
[AlexNet3]: https://raw.githubusercontent.com/weslynn/AlphaTree-graphic-deep-neural-network/master/modelpic/alexnet_data.png

### VGG

1. 论文["Very deep convolutional networks for large-scale image recognition"][VGG1]

2. VGG配置表

![][VGG2]

3. VGG19模型结构

![][VGG3]

see [more][VGG0] for details.

[VGG0]: https://github.com/weslynn/AlphaTree-graphic-deep-neural-network/blob/master/object%20classification%20%E7%89%A9%E4%BD%93%E5%88%86%E7%B1%BB/VGG.md
[VGG1]: https://arxiv.org/pdf/1409.1556.pdf
[VGG2]: https://raw.githubusercontent.com/weslynn/AlphaTree-graphic-deep-neural-network/master/pic/vgg.png
[VGG3]: https://raw.githubusercontent.com/weslynn/AlphaTree-graphic-deep-neural-network/master/modelpic/vgg19.png

参考代码:

```python
cfg = {
    "E": [
        64, 64, "M",
        128, 128, "M",
        256, 256, 256, 256, "M",
        512, 512, 512, 512, "M",
        512, 512, 512, 512, "M",
    ],
}

class VGG(nn.Module):
    def __init__(self, features, num_classes=1000, init_weights=True):
        super(VGG, self).__init__()
        self.features = features
        self.avgpool = nn.AdaptiveAvgPool2d((7, 7))
        self.classifier = nn.Sequential(
            nn.Linear(512 * 7 * 7, 4096),
            nn.ReLU(True),
            nn.Dropout(),
            nn.Linear(4096, 4096),
            nn.ReLU(True),
            nn.Dropout(),
            nn.Linear(4096, num_classes),
        )
        if init_weights:
            self._initialize_weights()

    def forward(self, x):
        x = self.features(x)
        x = self.avgpool(x)
        x = x.view(x.size(0), -1)
        x = self.classifier(x)
        return x

def make_layers(cfg):
    layers = []
    in_channels = 3
    for v in cfg:
        if v == "M":
            layers += [nn.MaxPool2d(kernel_size=2, stride=2)]
        else:
            conv2d = nn.Conv2d(in_channels, v, kernel_size=3, padding=1)
            layers += [conv2d, nn.ReLU(inplace=True)]
            in_channels = v
    return nn.Sequential(*layers)

def vgg19(**kwargs):
    return VGG(make_layers(cfg["E"]), **kwargs)

```

-----------------------------------------------------------------

## Object Detection

![det](https://github.com/weslynn/graphic-deep-neural-network/raw/master/map/ObjectDetection&Seg.png)

-----------------------------------------------------------------

## Object Segmentation

![seg](https://github.com/weslynn/graphic-deep-neural-network/raw/master/map/ObjectDetection&Seg.png)

-----------------------------------------------------------------

