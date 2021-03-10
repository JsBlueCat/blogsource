---
title: faceNet - A Unified Embedding for Face Recognition and Clustering
date: 2018-05-10 08:17:04
tags:
    - Face
categories: 
    - 3D
---

# A Unified Embedding for Face Recognition and Clustering
本文主要是根据很多的论文集合成了一个人脸识别的算法。 直接学习从人脸图像到欧式空间的映射。这个映射就等价于人脸的相似程度。只要学习了空间就可以完成人脸的识别，分类，聚类。

## 优点
使用了一个深度的卷积网络去优化了人脸认知的准确率，中间使用了128bit的脸部特征就行了，使用了。正确率达到了99.63%

## 简介
一旦空间学成，那么图片的识别就可以看作，距离阈值的取值，从而就可以变成knn,聚类问题。

之前的人脸识别的方法，是基于深度学习网络的 （classification layer）分类层 训练出人脸的特征，然后再用一个  bottleneck layer（瓶颈层）给出结果，这样做的缺点有 这个方法的不直接性，不准确性。 而且需要很多的维度，虽然有些算法用PCA 去投影降低了这个网络,但是这个线性变化完全可以用 1\*1的网络去学习出来。

## 做法
作者 直接训练一个 128的紧密欧式空间 基于LMNN 算法(上篇算法有介绍),然后样例有2个匹配的脸和 1 个不匹配的脸。 缩略图是脸部区域的紧凑作物，除了执行缩放和平移之外，没有2D或3D对齐。 定义一个三元损失函数，去优化距离。

他们加了几个 1\*1\*d 的卷积层，和一个 混合层(mixed layers) 和 池化层 对齐这些输出。

![](/images/faceNet/tripleLoss.png)
定义了一个三元损失函数,就直接反映了人脸的相似程度，作者发现 当L = 
![](/images/faceNet/L.png) 的时候 最优

## cnn模型
![](/images/faceNet/table.png) 的时候 最优