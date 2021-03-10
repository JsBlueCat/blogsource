---
title: Image2Mesh:A Learning Framework for Single Image 3D Reconstruction
date: 2019-07-08 07:35:11
tags:
    - Mesh
categories: 
    - 3D
---
# Image2Mesh: A Learning Framework for Single Image 3D Reconstruction

目前3d呈现数据中，都是使用体素，点云，类似的数据格式，但是这种方法有很大的弊端：
1. 计算复杂性
2. 数据无序性
3. 空间数据缺失

本文提出的想法是，直接从复杂的mesh呈现中回归出模型结构，而不是直接输出一个mesh结构

# 关键2大技术
1. FFD free-form deformation 自由变换
2. sparse linear combination 空间线性组合 
   
# 介绍
人类认知物体是通过图片和我们脑子中的图像还原3d的呈现方式。而本文的想法是基于很多的CAD 模型开源之后，
获取先验数据可能作为基础启发的这篇论文。

![](/images/img2mesh/1.png)