---
title: meshcnn
date: 2019-07-10 14:54:15
tags:
    - Mesh
categories: 
    - 3D
---

# MeshCNN: A Network with an Edge
本篇文章和以前传统的一些文章有很多不同的地方，本文提出使用特殊的卷积操作和池化操作，作用于mesh的边缘，通过减少空间的连接。

# 介绍

mesh 网格呈现是比其他同类型的数据呈现要高效（19年趋势，感觉套近乎）
优点是
1. 只需要很小格式的polygons
2. 旋转啊，变形啊，这种操作可以很好的执行

传统的CNN有一个很好的优势，是因为图片的呈现格式是基于一个网格，CNN应用于无规则数据上是没用的。（这也就是meshcnn 使用边 ， meshnet 使用中心点。）

meshcnn 选择使用边作为一个数据，因为边和2个三角相邻，作者使用一个对称卷积操作（不知道为什么要对称，对称会对结果产生影响吗？）


Since features are on the edges, an intuitive approach for downsampling is to use the well-known mesh simplification technique edge collapse [Hoppe 1997].

作者使用了1997年对于mesh 研究的collapse方法
![](/images/meshcnn/1.png)

# 先前的工作

classic mesh processing techniques [Hoppe 1999; Rusinkiewicz and Levoy 2000; Botsch et al. 2010; Kalogerakis et al. 2010]
mesh simplification techniques [Hoppe et al. 1993; Garland and Heckbert 1997; Hoppe 1997].
作者还是看了下传统的处理流程，用深度学习复现了 edge-collapse technique [Hoppe 1997] 算法。

1. 多维度图片 
2. 体素 
3. Graph 图
4. Manifold 多样呈现
5. 点云
   
The uniqueness of our approach compared to the previous ones is that our network operations are specifically designed to adapt to the mesh structure.
作者说，与先前最大的不同，是直接作用于mesh数据上的

# 方法 

## 卷积
上文已经阐述，作者将使用边作为输入数据。
**Invariant convolutions.** 
作者怎么保证卷积不变性，首先边以逆时针方向
![](/images/meshcnn/2.png)
如图中以e为边（a,b,c,d） , (c,d,b,a) 这两种不能解决旋转不变性。
作者让 ac ， bd 成对， 然后加入简单的 simple 的对称函数 比如 SUM(a,c)

**Input features**
对于输入特征 ![](/images/meshcnn/3.png) 作者定义了
1个二面角，2个内角和2个边垂比。
首先可以更具 二面角 -90 ° 还原一个内角，再有一个内角。就得到了3个内角
其次根据e边长的大小，与长宽比相乘，就还原了垂直距离。
最后通过排序，来解决旋转不变性。

**Global ordering**
作者使用pointnet的全局池化。使用原先的序列，保证旋转不变性。

**Pooling**
![](/images/meshcnn/4.png)
it provides flexibility with regards to the output dimensions of the pooling layer
作者的池化，用的是传统的mesh崩塌方法

# 方法
二维图像的网格，天然的定义了 周围连接的元素。
作者重新定义了一下mesh的格式
A mesh is defined by the pair (V , F ), where V = {v1, v2 · · · } is
the set of vertex positions in R3, and F defines the connectivity
(triplets of vertices for triangular meshes).

定义了 V 和 F 的集合  V是顶点坐标，F是三角网格的三边。V,F已知，定义E 为边的集合
边的集合使用了很多相关的特征。
mesh 拥有的特性，卷积的相邻的元素是原始空间输入特征.

# Mesh Convolution
![](/images/meshcnn/5.png)
(e1, e2, e3, e4) = (|a − c|, a + c, |b − d|,b + d) 此而保证了旋转不变性

general matrix multiplication
(GEMM): by expanding (or unwrapping) the image into
a column matrix (i.e., im2col [Jia 2014]).

矩阵的卷积 可以展开为列矩阵的乘法.

nc × ne × 5 feature tensor, nc 是 特征向量的维数,ne 是边的数量, 5 是上述 定义的边

Therefore, we can control the desired resolution of the mesh after
each pooling operator, by adding a hyper-parameter which defines
the number of target edges in the pooled mesh
加入一个超参数定义池化之后的边数
querying special
data structures that are continually updated (see [Berg et al. 2008]
for details).

# 实验
本实验配置比较简单所以就没用docker 直接conda一下
``` shell
conda activate meshcnn && bash ./scripts/shrec/train.sh
```
![](/images/meshcnn/7.png)
![](/images/meshcnn/8.png)

# 结果
![](/images/meshcnn/6.png)
在分类,segment(分割),还有human seg 上有特别好的效果