---
title: Research Records Week-1
mathjax: true
description: 第二周的科研学习记录。
date: 2024-07-08 23:28:35
tags:
- Unfinished
- 上科大
- 计算机科学
category:
- 科研
- 笔记
---

# 尝试深入理解 Encoder 和 Decoder 架构

- [Variational Autoencoders ](https://www.youtube.com/watch?v=9zKuYvjFFS8&t=70s)
    > In this episode, we dive into Variational Autoencoders, a class of neural networks that can learn to compress data completely unsupervised!
    > 
    > VAE's are a very hot topic right now in unsupervised modelling of latent variables and provide a unique solution to the curse of dimensionality.
    > 
    > This video starts with a quick intro into normal autoencoders and then goes into VAE's and disentangled beta-VAE's.
    > I aslo touch upon related topics like learning causal, latent representations, image segmentation and the reparameterization trick!
    - Encoder 本质上就是在 Compress Info ，它其实就和 CNN 中 进行 Convolution 和 Pooling 的过程是一样的，只不过使用 NN 来实现。这个特性很适合压缩数据。
    - Encoded 得到的 Latent Vector 抛弃了信息，而大部分都会是无用的信息，就和 PCA 里面对特征值进行降维一样。这个特性 很适合 Denoising Task。
    - Decoder 会利用得到的 Latent Vector 重构原来的数据，这部分也被称作是 Generative 的。
    - Disentangled VAE 做到了让 Latent Vector 中的每一个维度都对应一个特征（人眼中的特征，例如方向，角度），这样的 NN 解释性更强。


- [Transformer models: Encoder-Decoders](https://www.youtube.com/watch?v=0_4KEb08xrE&t=14s)
    > A general high-level introduction to the Encoder-Decoder, or sequence-to-sequence models using the Transformer architecture. What is it, when should you use it?
    - Encoder 和 Decoder 是两个独立的 NN，他们的权重独立

- [An introduction to variational autoencoders (VAE)](https://youtu.be/YV9D3TWY5Zo?si=DohqJ7OQEjI0kXK1)
    - 从 Latent Linear Space 中潜藏一大堆 Noise Latent Vector 出发解释为什么要用 VAE
    - VAE 就是让 Input 和 概率分布进行 Mapping，而不是和一个具体的 Vector 进行 Mapping。
    - Sequence to Sequence 的 NLP Task 中 Decoder 会进行一个 Loop Generation，也就是第一次只使用 Latent Vector，生成一个 Token 后，被生成的 Token 会被重新 Feed 到 Decoder 中，生成下一个 Token。这种架构被常用于更具体的 Summarization & Translation Task。 

-  [Variational Autoencoder - Model, ELBO, loss function and maths explained easily!](https://www.youtube.com/watch?v=iwEzwTTalbg)
    - 了解了很多 Mathematical 的细节：KuIIback-Leibler Divergence, ELBO, Reparameterization Trick, Loss Function.
    - KuIIback-Leibler Divergence 是用来衡量两个概率分布之间的差异的
    - ELBO 和 KL Divergence 一起构成了 VAE 的 Loss Function，由于 $KL\; Divergence \geq 0$，所以 ELBO 被叫做 "Evidence **Lower Bound**"，也就是 Loss Function 的下界。
