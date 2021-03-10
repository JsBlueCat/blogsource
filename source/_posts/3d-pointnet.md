---
title: Point Net 论文解读
date: 2019-05-31 12:20:02
tags:
    - Point Cloud
categories:
    - 3D
---

# Point Net 
本篇论文，是点云的开山之作，主要的贡献<p style='color:red'>直接输入点云序列，得到实例分割以及物体分类的相关序列</p>
效果类似于
![](/images/pointnet/1.png)

# 在之前3D 点云遇见的问题

## 无序性：
点云本质上是一长串点（nx3矩阵，其中n是点数）。在几何上，点的顺序不影响它在空间中对整体形状的表示，例如，相同的点云可以由两个完全不同的矩阵表示。
![](/images/pointnet/2.png)

解决方案：

x代表点云中某个点，h代表特征提取层，g叫做对称方法，r代表更高维特征提取，最后接一个softmax分类。g可以是maxpooling或sumpooling，也就是说，最后的D维特征对每一维都选取N个点中对应的最大特征值或特征值总和，这样就可以通过g来解决无序性问题。pointnet采用了max-pooling策略。
![](/images/pointnet/3.png)


## 旋转性：

相同的点云在空间中经过一定的刚性变化（旋转或平移），坐标发生变化

希望不论点云在怎样的坐标系下呈现，网络都能正确的识别出。这个问题可以通过STN（spacial transform netw）来解决。二维的变换方法可以参考这里，三维不太一样的是点云是一个不规则的结构（无序，无网格），不需要重采样的过程。pointnet通过学习一个矩阵来达到对目标最有效的变换。
![](/images/pointnet/4.png)


# 网络结构
![](/images/pointnet/5.png)
- 空间变换网络解决旋转问题：三维的STN可以通过学习点云本身的位姿信息学习到一个最有利于网络进行分类或分割的DxD旋转矩阵（D代表特征维度，pointnet中D采用3和64）。至于其中的原理，我的理解是，通过控制最后的loss来对变换矩阵进行调整，pointnet并不关心最后真正做了什么变换，只要有利于最后的结果都可以。pointnet采用了两次STN，第一次input transform是对空间中点云进行调整，直观上理解是旋转出一个更有利于分类或分割的角度，比如把物体转到正面；第二次feature transform是对提取出的64维特征进行对齐，即在特征层面对点云进行变换。
- maxpooling解决无序性问题：网络对每个点进行了一定程度的特征提取之后，maxpooling可以对点云的整体提取出global feature。

# 复线实验
pointnet官方是没有给docker 镜像的，本人直接给大家做好了镜像，直接跑就行了

``` shell
sudo docker pull jsbluecat/pytorch:pointnet
sudo docker run -it --runtime=nvidia --device=/dev/video0 -e DISPLAY=unix$DISPLAY -v /tmp/.X11-unix:/tpm/.X11-unix jsbluecat/pytorch:pointnet bsh
# 训练
cd utils && python train_segmentation --dataset ../shapenetcore_partanno_segmentation_benchmark_v0 --nepoch=200
# 显示结果
python show_seg.py --dataset ../shapenetcore_partanno_segmentation_benchmark_v0 -model ./seg/seg_model_Chair_199.pth --idx 50 --class_choice Chair
```
# 实验结果
![](/images/pointnet/6.jpg)
<video id="video" controls="" preload="none" poster="http://om2bks7xs.bkt.clouddn.com/2017-08-26-Markdown-Advance-Video.jpg">
    <source id="mp4" src="https://www.shuky.cn:8001/images/upload/1.mp4" type="video/mp4">
</video>

# 源码解读
面谈