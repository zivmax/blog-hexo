---
title: Research Records Week-1
mathjax: true
description: 第一周的科研学习记录，主要是基础学习和阅读论文。
date: 2024-07-03 20:08:35
tags:
- Unfinished
- 上科大
- 计算机科学
category:
- 科研
- 笔记
---

# 学习超声成像的基本原理

从知乎一篇[专栏文章](https://www.zhihu.com/search?type=content&q=%E8%B6%85%E5%A3%B0%E6%88%90%E5%83%8F%E5%8E%9F%E7%90%86)学的。

这篇文章是机翻[外网的文章](https://www.howequipmentworks.com/ultrasound_basics/)。

# 复习线性代数的本质

首先重新复习了一遍 3Blue1Brown 的线性代数系列，捡回了对于线性代数的基本理解：线性变换。

# 图形学入门

通过师兄给的 OpenGL 入门资料，学习了很多图形学的基础知识，包括：
- Basic Pipeline
- Coordinate System
- Camera
- Transformation
- Shape's Mesh Representation

# 复习神经网络

又看了一遍 3Blue1Brown 的神经网络系列，复习了神经网络的基本原理，捡起来了一些名词概念。

事实上感觉把吴恩达 DL full course 中的前两个 sections 学习一遍很有必要，后面读论文感觉到很多概念都临时学。

# 阅读文献

## 论文：DeepSDF

### 核心 Ideas：

####  Representing 3D Shape in SDF 
SDF 很适合用来表示 3D Shape，因为它是一个 Signed Distance Function，可以表示一个点到 Shape 的距离，同时可以表示点在 Shape 内部还是外部。

同时它是一个函数，这种形式很适合用来训练神经网络。

#### Auto-Decoder Architecture
文中提到可以用 Single DeepSDF，这样理论上就不需要任何 Encode 或 Decode，不过这样神经网络就只能 Embed 进一个 Shape。

使用 Coded DeepSDF 的话，可以提前 encode 多种 shapes 为 code，然后在 predict 的时候选择不同的 code，和特征一起投入 NN，这样就可以只 embed common properties into NN，然后通过不同的 code 表示不同的 Shape Prior。让网络具有泛用性。

而所谓 Auto-Decoder：

- 开始训练的时候，除了 Shape 的特征值以外，随机初始化一个 latent code，一起投入 NN
- 反向传播的时候 NN 也会对 latent code
- 最后收敛的时候，NN 不只是训练好了自己的 Weights，同时也训练好了 latent code
- 这个 latent code 被存起来
 
看起来就像是把 Encoder 的训练工作交给了 NN，让 NN 自己学习如何 encode 一个 Shape。

使用这种架构的原因大概率和推理效率有关，**不过还是比较存疑为什么要用这种架构**。

## 论文：Occupancy Networks

### 核心 Ideas：

#### Representing 3D Shape in Occupancy Grid

其实和 SDF 的概念非常像，两者区分 Shape surface 的办法都是通过一个连续函数输出一个值，这个值会表示在 Shape 内部还是外部。

Occupancy 就是训练了一个输入为三维位置的一个二分器，输出是一个概率值，表示这个点在 Shape 内部的概率。

不过在具体确定 Shape 的时候，SDF 是通过一个距离来确定，SD 等于 0 时表示在 Shape surface 上。

#### Reconstruction by Grid Test

而 Occupancy 通过一种“循环递归实验”的方式来确定：
- 首先测试一个稀疏，范围也最大的 Grid，看 Grid 上哪些点在 Shape 内部。
- 接着聚焦与这些点，再次测试一个范围更小的，但是密度更大的 Grid，看哪些点在 Shape 内部。
- 循环递归上面的步骤，直到 Grid 的密度足够大，就可以确定 Shape 的表面。

所以就是每次测试的点的数量一样多，但是范围越来越小，密度越来越大。密度大到一定程度，就可以确定 Shape 的表面。


## 论文：Neural Pull

### 核心 Ideas：

#### Trained on Point Cloud

前两篇论文都使用 Ground Truth WaterTight Shape 进行的训练，而 Neural Pull 使用的是 Point Cloud，不过训练的时候还是用的 Ground Truth 采样的 Point Cloud。

#### Pulling Points To Surface

Neural Pull 的核心思想是把 Point Cloud 上的点拉到 Shape 的表面上。

它的 Loss 就是 NN 输出的 Pulled Point 和 Ground Truth Surface Point 之间的距离。

要确定如何 Pull Point 要利用一个涉及梯度计算的简单公式：

$$t_i^{\prime}=q_i-f(c,q_i)\times\nabla f(c,q_i)/\|\nabla f(c,q_i)\|_2$$


## 论文：Learning Shape from Sparse

### 核心 Idea

#### Solve the Missing Information Caused by Sliced Data

当下的很多模型分辨率都很受限于用于训练的 Prior Shape 的分辨率。

而医学成像中，大量的数据都是断层切片，切片的密度会极大地影响到 Shape 的分辨率。

这篇论文尝试解决这个问题


#### Occupancy Solves the Slice Distances Problem

然而，对于 Segmentation Mask 之间的切片距离，缺失的 Surface 让我们无法精确估计 Surface Distance，因为没有一个 Watertight Shape。

 Occupancy 则比较适合这种数据，因为它只需要一个点在 Shape 内部的概率，而不需要精确的 Surface Distance。

#### Hybrid Loss

训练 Implicit Representations on Continuous surface ，常用 binary cross-entrop loss。

对于 Voxel-based Data，混合使用 binary cross-entrop 和 Soft Dice 被证实是 well perferomance 的。

所以本文使用的就是这个 hybrid loss。

$$L_d(\hat{Y}_i,Y_i)=1-\frac{2\sum_{j=1}^M\hat{y}_i^j\cdot y_i^j}{\sum_{j=1}^M(\hat{y}_i^j+y_i^j)},$$

combined with the voxel-wise binary cross-entropy loss $H(\cdot,\cdot):$
$$L(\hat{Y}_i,Y_i)=L_d(\hat{Y}_i,Y_i)+\frac{1}{M}\sum_{j=1}^MH(\hat{y}_i^j,y_i^j).$$

#### Innovative Validation

由于采用的是 Auto-Decoder 架构，并且所有的 Reconstruction 需要一个 Latent Vector 作为 Shape Prior，常见的划分 Validation Set & Training Set 的方法就不是很合适。

所以这篇论文使用了一种独特的 Validation：

For the Shapes wanted to be trained:
- Use every second slice as Training Set
- Rest slices as Validation Set
- When validating, use the Validation Set to predict, which is actually validating is the NN has learned the Shape Prior.



