---
title: darts
date: 2018-07-03 07:28:32
tags:
    - 自架构搜索网络
categories: 
    - 强化学习
---

# DARTS: Differentiable Architecture Search

## 指数级加速架构搜索：CMU提出基于梯度下降的可微架构搜索方法

与传统最优架构不同，DARTS提出了自学架构的方式

研究者称，
该方法已被证明在卷积神经网络和循环神经网络上都可以获得业内最优的效果，
而所用 GPU 算力有时甚至仅为此前搜索方法的 700 分之 1，这意味着单块 GPU 也可以完成任务。
该研究的论文《DARTS: Differentiable Architecture Search》
一经发出便引起了 Andrew Karpathy、Oriol Vinyals 等学者的关注。

# 架构搜索在CNN 与 RNN 上的演示

![](https://user-gold-cdn.xitu.io/2018/6/28/16444fbdf4b1e632?imageslim) 

![](https://user-gold-cdn.xitu.io/2018/6/28/16444fc14e4af4d6?imageslim)

## 对比

1.当前最佳的架构搜索算法尽管性能优越，但需要很高的计算开销。例如，在 CIFAR-10 和 ImageNet 上获得当前最佳架构需要强化学习的 1800 个 GPU 工作天数 (Zoph et al., 2017) 或进化算法的 3150 个 GPU 工作天数（Real et al., 2018）。 原因是因为架构搜索被当成一种在离散域上的黑箱优化问题，这导致需要大量的架构评估。

2.高效架构搜索方法 DARTS（可微架构搜索）。该方法不同于之前在候选架构的离散集上搜索的方式，而是将搜索空间松弛为连续的，从而架构可以通过梯度下降并根据在验证集上的表现进行优化。与基于梯度优化的数据有效性和低效的黑箱搜索相反，它允许 DARTS 使用少几个数量级的计算资源达到和当前最佳性能有竞争力的结果。它还超越了其它近期的高效架构搜索方法 ENAS（Pham et al., 2018b）。

## 成就

- 使用 4 块 GPU：经过 1 天训练之后在 CIFAR-10 上达到了 2.83% 的误差；经过六小时训练后在 PTB 达到了 56.1 的困惑度，研究者将其归功于基于梯度的优化方法
- 在图像分类和语言建模任务上进行的大量实验表明：基于梯度的架构搜索在 CIFAR-10 上获得了极具竞争力的结果，在 PTB 上的性能优于之前最优的结果。这个结果非常有趣，要知道目前最好的架构搜索方法使用的是不可微搜索方法，如基于强化学习（Zoph et al., 2017）或进化（Real et al., 2018; Liu et al., 2017b）的方法。

# 原理剖析
![](https://user-gold-cdn.xitu.io/2018/6/28/16444fc14f7b5e90?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

图 1：DARTS 概述：（a）一开始并不知道对边缘的操作。（b）通过在每个边缘放置各种候选操作来连续放宽搜索空间。（c）通过求解二级（bilevel）优化问题，联合优化混合概率和网络权重。（d）从学习到的混合概率中归纳出最终的架构。

基于松弛空间，连续优化。（目前还不了解这里的数学理论，需要进一步研究）

## 实验设备
实验的所有设备都只基于一块 NVIDIA GTX 1080Ti
![](/images/darts/3.png) 
实验设别对比 结果发现   TITAN V 略逊色 Tesla  远超 1080ti 
Tesla  >  TITAN V  > 1080 Ti
# 算法
![](/images/darts/1.png) 

![](/images/darts/2.png) 
这里需要求2阶混合偏导数
![](/images/darts/4.png) 
他把混合偏导，用导数定义法，近似成双边切线求导。
复杂度 从 O(a*w) 降为  O(a + w) 

# 与其他算法对比
![](https://user-gold-cdn.xitu.io/2018/6/28/16444fc168f2038a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
当前最优图像分类器在 CIFAR-10 上的对比结果。带有 † 标记的结果是使用本论文设置训练对应架构所得到的结果。

# 源码分析
```python
from collections import namedtuple

Genotype = namedtuple('Genotype', 'normal normal_concat reduce reduce_concat')

PRIMITIVES = [
    'none',
    'max_pool_3x3',
    'avg_pool_3x3',
    'skip_connect',
    'sep_conv_3x3',
    'sep_conv_5x5',
    'dil_conv_3x3',
    'dil_conv_5x5'
]

NASNet = Genotype(
  normal = [
    ('sep_conv_5x5', 1),
    ('sep_conv_3x3', 0),
    ('sep_conv_5x5', 0),
    ('sep_conv_3x3', 0),
    ('avg_pool_3x3', 1),
    ('skip_connect', 0),
    ('avg_pool_3x3', 0),
    ('avg_pool_3x3', 0),
    ('sep_conv_3x3', 1),
    ('skip_connect', 1),
  ],
  normal_concat = [2, 3, 4, 5, 6],
  reduce = [
    ('sep_conv_5x5', 1),
    ('sep_conv_7x7', 0),
    ('max_pool_3x3', 1),
    ('sep_conv_7x7', 0),
    ('avg_pool_3x3', 1),
    ('sep_conv_5x5', 0),
    ('skip_connect', 3),
    ('avg_pool_3x3', 2),
    ('sep_conv_3x3', 2),
    ('max_pool_3x3', 1),
  ],
  reduce_concat = [4, 5, 6],
)
    
AmoebaNet = Genotype(
  normal = [
    ('avg_pool_3x3', 0),
    ('max_pool_3x3', 1),
    ('sep_conv_3x3', 0),
    ('sep_conv_5x5', 2),
    ('sep_conv_3x3', 0),
    ('avg_pool_3x3', 3),
    ('sep_conv_3x3', 1),
    ('skip_connect', 1),
    ('skip_connect', 0),
    ('avg_pool_3x3', 1),
    ],
  normal_concat = [4, 5, 6],
  reduce = [
    ('avg_pool_3x3', 0),
    ('sep_conv_3x3', 1),
    ('max_pool_3x3', 0),
    ('sep_conv_7x7', 2),
    ('sep_conv_7x7', 0),
    ('avg_pool_3x3', 1),
    ('max_pool_3x3', 0),
    ('max_pool_3x3', 1),
    ('conv_7x1_1x7', 0),
    ('sep_conv_3x3', 5),
  ],
  reduce_concat = [3, 4, 6]
)

DARTS = Genotype(normal=[('sep_conv_3x3', 1), ('sep_conv_3x3', 0), ('skip_connect', 0), ('sep_conv_3x3', 1), ('skip_connect', 0), ('sep_conv_3x3', 1), ('sep_conv_3x3', 0), ('skip_connect', 2)], normal_concat=[2, 3, 4, 5], reduce=[('max_pool_3x3', 0), ('max_pool_3x3', 1), ('skip_connect', 2), ('max_pool_3x3', 0), ('max_pool_3x3', 0), ('skip_connect', 2), ('skip_connect', 2), ('avg_pool_3x3', 0)], reduce_concat=[2, 3, 4, 5])
```

可以看到是把所有结构都对应成数字，然后梯度下降学习这些数字，然后变化对于架构。

# 开发环境 pytorch

pytorch  -> onnx -> tensorflow -> tensorflow.js

各模型在各种场合的使用