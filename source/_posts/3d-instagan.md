---
title: instagan
date: 2019-06-12 17:09:34
tags:
    - GAN
categories: 
    - 3D
---

# instance-aware GAN (InstaGAN)
先前的GAN 模型在 形状转换上有很多不足.
本文提出的gan 在综合 实例信息 和 物体风格的mask 矩阵结合

本文提出 context preserving loss

3个共享 
1. 实例增强的网络架构
2. 内容呈现损失
3. 连续mini-batch 训练技术

# 做法
1. 首先提出一个网络 翻译 图片和相关数据集的实例
2. 然后提出多实例的损失
3. 最后提出单一gpu 的 mini batch 训练