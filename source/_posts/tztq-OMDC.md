---
title: OMDC
date: 2018-09-04 19:57:27
tags:
        - machine learning
categories: 
		- 特征提取
---

# Optimal Margin Distribution Clustering
优化边缘分布的一种聚类方法

## 最大边际聚类（MMC)
最大边际聚类（MMC）借用了支持向量机（SVM）的大边缘启发式.
最大最小边缘不一定 提升准确率 ， 而是要选取好的边缘分布

## 优缺点 
在本文中，我们提出一种新颖的方法ODMC（用于聚类的最佳边际分布机），它试图聚类数据并同时实现最佳边际分布。
具体来说，我们通过一阶和二阶统计量（即边际均值和方差）来表征边际分布，并扩展随机镜下降法以解决最终的极小极大问题

## 提出问题
发现 MMC 对于 边缘的分布的依赖，比边缘距离更加的重要

# 机器学习的地位
聚类是机器学习的一个重要研究领域，旨在分组的数据挖掘和模式识别，数据点相似。

# 聚类算法的发展史
- MMC 方法 最大svm 距离
- 半SDP 凸优化 使用半正定矩阵来限制边缘
- tighter minimax relaxation  通过迭代生成最违反的标签来解决，然后通过多核学习将它们组合起来。
- alternative optimization 通过顺序查找标签和优化支持向量回归（SVR）来执行聚类，并且约束凹凸程序
- 对mmc 进行改进(ODM)   优化边缘分布 
- zhou 等人 提出了 exploit unlabeled data and handle unequal misclassification cost.
- 对于 MMC 提出了增强

# 作者提出的算法 ODMC 
在本文中，作者提出了一种新的方法——ODMC（Optimal margin Distribution Machine for Clustering，用于聚类的最优间隔分布机），该方法可以用于聚类并同时获得最优间隔分布。特别地，他们利用一阶和二阶统计量（即间隔均值和方差）描述间隔分布，然后应用 Li 等人 2009 年提出的极小极大凸松弛法（已证明比 SDP 松弛法更严格）以获得凸形式化（convex reformulation）。作者扩展了随机镜像下降法（stochastic mirror descent method）以求解因而产生的极小极大问题，在实际应用中可以快速地收敛。此外，我们理论上证明了 ODMC 与当前最佳的割平面算法有相同的收敛速率，但每次迭代的计算消耗大大降低，因此我们的方法相比已有的方法有更好的可扩展性。在 UCI 数据集上的大量实验表明 ODMC 显著地优于对比的方法，从而证明了最优间隔分布学习的优越性。

## 实现途径
使用第一和第二统计 来标记边缘分布，边缘的均值和变量。
svm 可以用来 最大或者最小训练数据的距离 。用来选择 hx 函数  就一个权重和feature map的乘积
导致的结果就似乎 svm 只包括一些数据  有 支持向量， 其余的都被忽略了，可能会 误导决策

![](/images/odmc/3.png) 
![](/images/odmc/4.png) 
利用如上2个公式 使用了 surrogate loss  分割 svm 的支持向量的 结果 
将原先的凸优化  转变成 由{0，1} 组成的 混合组成优化

# 伪代码思想 
![](/images/odmc/1.png) 
![](/images/odmc/2.png) 

# 感想
机器学习，这块使用了大量的 统计方法，使得同分布。
这篇文章巧妙的将svm 分类之后的 边缘点 符合一种分布。最终使用  镜像梯度下降的方法。得到相同的准确率，但提升了迭代的效率