---
layout:     post
title:      “【R】FeatherNets: Convolutional Neural Networks as Light as Featherfor Face Anti-spoofing"
date:       2019-09-21 00:00:00
author:     "Tp"
header-img: "img/post-bg-2015.jpg"
tags:
    - CV
    - Paper
---





## Introduction

为了解决需要计算量和内存的问题，设计一个轻量级的CNN结构，同时设计了一个叫做Streaming module的结构，它在准确率的表现上比GAP要好很多。

## Approach

### -->Sreaming module

> GAP虽然在目标检测上可以降低过拟合，但是在人脸检测上，由于不同人脸部分的重要程度不同，因此提出了使用深度分离卷积(DWConv)来代替GAP，同时也去掉了全连接层，让这个网络更加轻量级。

![](/Users/tianpeng/Documents/myblog/img/feathernets/GAP&DWC.png)

Streaming module 是由一个深度分离卷积，并采用步长超过1的核进行下采样，然后再压缩成一个一维的特征向量：

![](/Users/tianpeng/Documents/myblog/img/feathernets/Streaming module.png)

计算公式如下:

![](/Users/tianpeng/Documents/myblog/img/feathernets/Steaming_module_compute_equation.png)

等式左边_FV_是被压缩的特征向量，具有_N=H’xW'xC_个元素，其中：

![](/Users/tianpeng/Documents/myblog/img/feathernets/Streaming_module_nth.png)

在上式中坐标为（y,x）在右边K是深度分离卷积的核，F是最初始的FeatureMap，
