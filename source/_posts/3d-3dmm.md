---
title: Fitting 3D Morphable Models using Local Features 解读
date: 2019-06-01 19:43:48
tags:
    - Face
categories: 
    - 3D
---

# 简介
使用局部特征来适用3D的模型. 2D的人脸.
为了解决特征提取器不可微分的特性,本文引入了一个基于数据学习的级联回归方法.
本方法是一个可以应用在实际上的方法

# 遇见的问题 
1. 现有的拟合方法话费太大时间.  但是大多他们使用的元素是 landmark,简单的特征. 和颜色信息, 以及 边缘分割图.
2. 但是还没有人用过,HoG 和 旋转尺度不变 特征来检测, 因为他们不可微.

# 解决方案 
本文使用 SIFT 的特征 和一个级联回归网络 有2个比较好的特征

# 论文的布局
级联检测方法来优化可变模型的局部特征


# 实验结果
实验配置很麻烦,需要改CUDA 换 显卡的架构 一堆的C++文件
和往常一样,本人已经为大家做好了 docker 拉一下就行
``` sheel
sudo docker pull jsbluecat/3d:4dface
sudo docker run -it --runtime=nvidia --device=/dev/video0 -e DISPLAY=unix$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jsbluecat/3d:4dface bash
```

<video id="video" controls="" preload="none" poster="http://om2bks7xs.bkt.clouddn.com/2017-08-26-Markdown-Advance-Video.jpg">
    <source id="mp4" src="https://www.shuky.cn:8001/images/upload/3dmm.mp4" type="video/mp4">
</video>
