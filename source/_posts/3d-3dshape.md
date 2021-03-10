---
title: 斯坦福图形学入门学习01
date: 2019-06-21 15:34:56
categories: 
    - 3D
---

# CS468 入门
随着目前计算机的飞速发展,3d方面渐渐的越来越多的深入我们的生活,本教程将带领大家慢慢的接触3d方向的内容,但是本人是一个刚接触3d的小白,如果有讲的不对的地方,还请大家见谅.

# shape analysis
第一章: 图形分析
本教程是mit 的相关教程  可以访问 ![gdp.csail.mit.edu/6838_spring_2017.html] 查看

## geometric data analysis
1. geometric data 作为 修饰词  叫做 地理坐标系数据的分析
2. geometric 作为修饰词 叫做 使用地理方法来分析数据

## aplied geometric 相关概念
1. 理论工具
2. 计算工具
3. 应用区域


### Euclidean Geometry
欧式几何

Riemannian viewpoint 黎曼观点
观察一个物体的测度,在地理坐标系中,只需要一个角度和距离就行
当正曲率的时候 3维的物体是封闭的  负曲率为发散的.

### Triangle mesh
引自危机百科
A triangle mesh is a type of polygon mesh in computer graphics. It comprises a set of triangles (typically in three dimensions) that are connected by their common edges or corners.

Many graphics software packages and hardware devices can operate more efficiently on triangles that are grouped into meshes than on a similar number of triangles that are presented individually. This is typically because computer graphics do operations on the vertices at the corners of triangles. With individual triangles, the system has to operate on three vertices for every triangle. In a large mesh, there could be eight or more triangles meeting at a single vertex - by processing those vertices just once, it is possible to do a fraction of the work and achieve an identical effect. In many computer graphics applications it is necessary to manage a mesh of triangles. The mesh components are vertices, edges, and triangles. An application might require knowledge of the various connections between the mesh components. These connections can be managed independently of the actual vertex positions. This document describes a simple data structure that is convenient for managing the connections. This is not the only possible data structure. Many other types exist and have support for various queries about meshes.

三角网格是一种空间网格系统,它由多个三角型组成(尤其是三维),由角和边组成.

许多图形软件包和硬件设备可以在分组为网格的三角形上比在单独呈现的相似数量的三角形上更有效地操作。这通常是因为计算机图形对三角形拐角处的顶点进行操作。对于单个三角形，系统必须在每个三角形的三个顶点上操作。在大型网格中，可能有八个或更多个三角形在单个顶点相遇 - 通过仅处理这些顶点一次，可以完成一部分工作并实现相同的效果。在许多计算机图形应用程序中，必须管理三角形网格。网格组件是顶点，边和三角形。应用程序可能需要了解网格组件之间的各种连接。可以独立于实际顶点位置来管理这些连接。本文档描述了一种便于管理连接的简单数据结构。这不是唯一可能的数据结构。存在许多其他类型并且支持关于网格的各种查询。

### Triangle soup
When the faces of a polygon mesh are given but the connectivity is unknown, we must deal with of a polygon soup.

Polygon soup does not have any connectivity (each point has as many occurences as the number of polygons it belongs to).

### Graph
图
### Point cloud
点云
### Pairwise distance matrix
成对距离矩阵


## 扁平三角网格组成的图像
### 三角网格能有曲率吗?
![](/images/3d/1.png)

### convergence and structure
拉普拉斯变换不适应所有的变换

### numerical pde
数值偏微分方程
### smooth optimization
平滑优化
### discrete optimization
离散优化