---
title: Distance Metric Learning for Large Margin Nearest Neighbor Classification
date: 2018-05-08 08:08:53
tags:
        - machine learning
categories: 
        - 特征提取
---

# 1. 简介

本文在马氏矩阵的基础上，kNN 是依赖于计算距离的矩阵。本文引入了 Mahalanobis distance metric 马氏距离矩阵。本方法基于，矩阵k阶相近的是相同的类别，不同的类别有着很大的距离，提出了新的分类LMNN对于knn的错误率有很大的优化

## 1.1. 优点
本方法不同于 SVM。在多聚类方面，不需要任何的修正和扩展函数   

## 1.2. 做法
本文的方法增强了kNN的聚类效果，
有时如果对 训练数据 先分类，基于每个分类学习出一个个性矩阵，这样能更好的增加聚类效果


# 2. 介绍

kNN的分类效果 很大程度基于 他的距离函数。如果没有先验标签，KNN 基本上都是基于 欧氏距离 （假设是向量输入）。
但是欧式距离没有考虑标签的统计相关，每个欧式相关会事先定义，就是2个相同的分类，相同的抽象数据，他们的距离函数都可能不相似。

作者发现 kNN 可以被 根据标签学习一个合适的距离矩阵 去优化

## 2.1. 算法特点

增加训练样本的数量 通过一个 对欧式距离的线性变换。
线性变化 派生出 一个损失函数 
第一个惩罚距离 是k阶邻近的距离 
第二个 是2个不匹配标签的距离 
这样一弄 增加了 标签匹配的k阶邻近样本空间 
欧式距离  在新的样本空间上 可以被看做一个 马氏距离 
本文就提出了 这种邻近。

## 2.2. 背景知识

### 2.2.1. Distance Metric Learning

首先距离向量要符合如下定义
![](/images/LMNN/lmnn-1.png)

### 2.2.2. 距离投影
使用PCA
$$
\overrightarrow {x'}=L\overrightarrow {x}
$$
先把输入矩阵x 投影成  \\(x'\\) 这里的投影矩阵 L 要符合 \\(M=L^{T}L\\) M要是个正定阵

![](/images/LMNN/lmnn-2.png)
那么这个距离矩阵的定义就可以这么写了。 作者把这种形式的伪度量称为Mahalanobis度量。

### 2.2.3. 基于特征值优化

类似我2DPCA篇介绍的 投影计算迹，很大程度上和特征向量相关

### 2.2.4. linear discriminant analysis
LDA 计算类间和类内的协方差矩阵
![](/images/LMNN/lmnn-3.png)

然后 优化距离
![](/images/LMNN/lmnn-4.png)

# 3. 模型
本算法主要是把同类的距离拉近，异类的距离拉远
![](/images/LMNN/lmnn-5.png)
具体的损失函数定义如下：
![](/images/LMNN/lmnn-6.png)
![](/images/LMNN/lmnn-7.png)
总体的损失函数
![](/images/LMNN/lmnn-8.png)
作者说  这个\\(\mu\\) 取0.5就可以，无所谓


Generally, the parameter μ can be tuned via cross validation, though in our experience, the results
from minimizing the loss function in Eq. (13) did not depend sensitively on the value of μ. In
practice, the value μ = 0.5 worked well.

## 3.1. 测试误差
![](/images/LMNN/lmnn-9.png)

## 3.2. 聚类
![](/images/LMNN/lmnn-10.png)

