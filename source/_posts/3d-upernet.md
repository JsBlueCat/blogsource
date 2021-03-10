---
title: upernet
date: 2018-09-13 13:15:01
tags:
    - deep learning
categories: 
    - 3D
---

# Unified Perceptual Parsing for Scene Understanding
旷视科技提出统一感知解析网络UPerNet，优化场景理解

# 解决的问题
能够识别一个场景中突出物体的检测，和优化纹理和理解。

## 导语
人类对世界的视觉理解是多层次的，可以轻松分类场景，检测其中的物体，乃至识别物体的部分、纹理和材质。在本文中，旷视科技提出一种称之为统一感知解析（Unified Perceptual Parsing/UPP）的新任务，要求机器视觉系统从一张图像中识别出尽可能多的视觉概念。同时，多任务框架 UPerNet 被提出，训练策略被开发以学习混杂标注（heterogeneous annotations）。旷视科技在 UPP 上对 UPerNet 做了基准测试，结果表明其可有效分割大量的图像概念。这一已训练网络进一步用于发现自然场景中的视觉知识。

## 直观解释
![](/images/upernet/1.png) 

# 怎么解决的

1.我们提出了一个新的解析任务Unified Perceptual Parsing，它要求系统一次解析多个视觉概念

2.我们提出了一个名为UPerNet的新型网络，它具有层次结构，可以从多个图像数据集中学习异构数据。

3.该模型显示能够联合推断和发现图像下方丰富的视觉知识。

## 结构
![](/images/upernet/2.png) 

### FPN
![](/images/upernet/3.jpg) 
使用特征金字塔的方式

```python
 # FPN Module
self.fpn_in = []
for fpn_inplane in fpn_inplanes[:-1]: # skip the top layer
    self.fpn_in.append(nn.Sequential(
        nn.Conv2d(fpn_inplane, fpn_dim, kernel_size=1, bias=False),
        SynchronizedBatchNorm2d(fpn_dim),
        nn.ReLU(inplace=True)
    ))
self.fpn_in = nn.ModuleList(self.fpn_in)

self.fpn_out = []
for i in range(len(fpn_inplanes) - 1): # skip the top layer
    self.fpn_out.append(nn.Sequential(
        conv3x3_bn_relu(fpn_dim, fpn_dim, 1),
    ))
self.fpn_out = nn.ModuleList(self.fpn_out)
```

### PPM

![](/images/upernet/3.png) 

```python
 # PPM Module
self.ppm_pooling = []
self.ppm_conv = []

for scale in pool_scales:
    self.ppm_pooling.append(nn.AdaptiveAvgPool2d(scale))
    self.ppm_conv.append(nn.Sequential(
        nn.Conv2d(fc_dim, 512, kernel_size=1, bias=False),
        SynchronizedBatchNorm2d(512),
        nn.ReLU(inplace=True)
    ))
self.ppm_pooling = nn.ModuleList(self.ppm_pooling)
self.ppm_conv = nn.ModuleList(self.ppm_conv)
self.ppm_last_conv = conv3x3_bn_relu(fc_dim + len(pool_scales)*512, fpn_dim, 1)
```

# 文章总结
本文改进了 pspnet 的结构，加上了FPN 和 PPM 和 fusion 取得了良好的视觉效果