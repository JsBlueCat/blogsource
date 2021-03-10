---
title: MeshNet Mesh Neural Network for 3D Shape Representation
date: 2019-06-26 09:20:32
tags:
    - Mesh
categories: 
    - 3D
---


# MeshNet Mesh Neural Network for 3D Shape Representation

Mesh 是计算机图形学中一种特别常用的格式，一般叫做网格，如：特殊的三角网格。

本篇论文作者提出一个MeshNet 的网络，从而直接从Mesh数据中学习3D的呈现。比原先的数据格式达成的任务更好用。

# 介绍
对于3D 物体来说，总体来说有4种呈现方式
1. volumetric grid
2. multi-view
3. point cloud
4. mesh

## 前期工作
pointnet 提出了 一个Multi-Layer-Perceptron （MLP）操作 和一个对称函数。 详见我pointnet论文
![](/images/meshnet/1.png)

## mesh 之前的工作
[Kazhdan, Funkhouser, and Rusinkiewicz 2003] Kazhdan,
M.; Funkhouser, T.; and Rusinkiewicz, S. 2003. Rotation
Invariant Spherical Harmonic Representation of 3D Shape
Descriptors. In Symposium on Geometry Processing,
volume 6, 156–164.
只有 2003 年的 SPH 使用手写特征的方法。

## mesh 的 特征
mesh 是一个集合，拥有点，边，表面的3D存储格式。Mesh 数据有复杂和不规律性

1. 复杂性体现在，mesh数据有很多不同类型的集合和很多的元素
2. 不规律性体现在，mesh数据对于3D物体来说，有很多的数字

总的来说，mesh数据中带有很多的信息，这些信息是比其他类型都要有效，所以在分类和识别的任务中可能会有更好的效果

# 解决办法
作者将边和面的信息作为共享信息，从而做成了一个对称的方程。
作者提出神经网络对于网格3D 数据，并设计了一个层来抓取和聚合 polygon faces 的特征

##  原先之前的工作
1. 基于 Voxel的模型  比如  3DShapeNets VoxNet FPNN Vote3D OCNN 
2. 基于 View 的方式  MVCNN GVCNN 
3. 基于 point 的方式 PointNet PointNet++  SO-Net  kernel correlation
network PointSIFT Kd-Net
4. 混合方法  FusionNet PVNet

# meshnet 的设计方式 
作者先介绍了 mesh 数据 和分析他的属性，mesh 数据 是 一个 点，边，以及表面的集合。
mesh数据，在3D的展示方面对其他的类型的数据有很强的优越性， mesh 数据会损失更多的数据。
![](/images/meshnet/2.png)

所有的数据，使用了一个 角落，中心，邻边，以及法向量。
对于这样的网格复杂的问题，作者提出了2个想法。
为了减小数据组织，所有的单元，定义其联通，如果他们有一个相同的边，这样做有很多好处，第一简化了边之间的连接，使用一个对称的函数就可以解决无序性的问题。

##  特点
对于点来说，只需要知道空间位置，但是对于面的数据，我们不仅需要知道 空间位置，而且想知道 形状信息。 所以我们将 mesh 数据 分割成 **空间特征** 以及 **结构特征**。

1. 表面信息
   - 中心点
   - 角向量
   - 法向量
2. 邻边信息
   - 邻边下标

作者设计了2个模块分别适应**空间信息**和**结构信息**，并且设计了mesh 卷积块 去聚合信息
最后一个池化操作生成全局操作。

## 空间和结构操作
作者将表面信息，分为空间信息和结构信息。
### Spatial descriptor 空间信息
作者还是使用MLP的方式 类似point could 里面的方式。

### Structural descriptor: face rotate convolution 表面旋转卷积 
其原理类似于 一个卷积操作 
使用了 1/3（f(v1,v2)+f(v2,v3)+f(v1,v3)） 作为输出
![](/images/meshnet/3.png)

### Structural descriptor: face kernel correlation 表面关联

KCNet (Shen et al. ), which uses kernel correlation (KC) (Tsin and Kanade 2004) 
使用了KC 的核 
![](/images/meshnet/4.png)
公式中，使用了高斯核，目的是为了找到 第i个面与第k个核之间的距离最小

## mesh convolution
![](/images/meshnet/5.png)
作者设计了3种对称方程，最后发现 接MLP的那个最好

## 实验结果
和往常一样做好了docker 啦一下就行
``` shell
sudo docker run --rm -it --runtime=nvidia --device=/dev/video0 --shm-size 16G -e DISPLAY=unix$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v /home/cery/workspace/meshnet:/meshnet  jsbluecat/pytorch:meshnet bash
```
![](/images/meshnet/6.png)
模型展示
![](/images/meshnet/7.png)
![](/images/meshnet/8.png)