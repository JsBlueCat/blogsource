---
title: SplineCNN:Fast Geometric Deep Learning with Continuous B-Spline Kernels
date: 2019-07-15 11:27:56
tags:
    - Graph Convolution
categories: 
    - 3D
mathjax: true
---

# SplineCNN: Fast Geometric Deep Learning with Continuous B-Spline Kernels

本文提出条样方法的卷积神经网络,处理无规则和无序的空间数据,如mesh.
所谓的条样卷积:就是使用连续的核函数,以固定数量的训练权重.
并且splinecnn 允许段到段的训练,仅仅只用空间结构作为输入.

# 介绍
目前大多数深度学习方法的成功,来源于卷积操作,因为卷积有局部连接,权重共享,旋转不变性.
这些卷积层,很难作用到非欧领域类似离散展开和图.
![](/images/splinecnn/1.png)
目前有2个领域,一个是对光谱的处理,另外一个是对空间结构的处理.

# 相关工作
## Deep learning on graphs
## Local descriptors for discrete manifolds
## Spatial continuous convolution kernels

# SplineCNN
![](/images/splinecnn/2.png)

## Input graphs

G = (V,E,U) G 表示图结构,V 表示定点结构 V = { 1, ..., N } N 维的结构,E 表示边结构 为 V\*V 维度,
U 为 [0,1] N\*N\*d纬度的类似临接矩阵. 1 是 (i,j) 属于变 0 是不属于.

## Input node features

$f : V → R^{M_{in}}$ $f$ 是一个 函数的映射 从V的维度到 $f(i) ∈ R^{M_{in}}$ 最后输入特征就是这些函数的映射.

## B-spline basis functions
let ((Nm1,i)1≤i≤k1, . . . ,(Nmd,i)1≤i≤kd)
denote d open B-spline bases of degree m, based on uniform, i.e. equidistant, knot vectors (c.f . Piegl et al. [19]),
with k = (k1, . . . , kd) defining our d-dimensional kernel size.

# 3.2 主要概念
f(i) 代表不规则的空间结构，空间的信息可以被 U 表示

- graphs 对于图来说 V 和 E 已经有了 U 可以包含边缘的权重。  for example, features like the node degree of the target nodes.
- discrete manifolds V 包含离散的展开 E 代表欧式临接 U 包含关系 比如 极坐标 球形， 3维坐标。对于输入和输出每个边。

Therefore, meshes, for example, can be either interpreted as embedded three-dimensional graphs or as two-dimensional manifolds, 

# 3.3 Convolution operator
![](/images/splinecnn/3.png)
和传统卷及类似。
![](/images/splinecnn/4.png)
