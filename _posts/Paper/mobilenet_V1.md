---
layout:     post
title:      “【R】MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications"
date:       2019-10-09 24:00:00
author:     "Tp"
header-img: "img/post-bg-2015.jpg"
tags:
    - CV 
    - Paper
---

> 为了构建非常小的**低延迟**模型，可以轻松地匹配移动和嵌入式视觉应用的设计要求，这篇论文提出：

- an efﬁcient network architecture 
- a set of two hyper-parameters

## 前置内容

### 1.Depthwise Separable convolutions(深度可分离卷积)

### 2.Factorized Networks

### 3.Xception-->Inception V3

### 4.Squeezentnet

### 5.Structured transform networks and Deep fried convnets(结构变换网络)







## Depthwise Separable Convolution

> 首先它使用深度可分离的卷积来打破输出通道数量与内核大小之间的相互作用。将标准卷积分解为深度卷积和称为逐点卷积的1✖️1卷积

标准卷积的计算成本为

![](/img/mobilenet/标准卷积计算成本.png)

Dk是核的大小；Df是特征映射的大小；M是输入通道数；N是输出通道数

深度卷积每个输入通道应用单个卷积核，计算成本为：

![](/img/mobilenet/深度卷积计算成本.png)



However it only ﬁlters input channels, it does not combine them to create new features.

所以接下来要使用**1✖️1**逐点卷积，计算成本比较：

![](/img/mobilenet/计算成本比较.png)



其中mobilenet使用的是**3✖️3**的卷积核，运算速度比标准卷积小8至9倍，而精度降低很少

![](/img/mobilenet/深度分离卷积图示.png)



## Width multiplier







## Resolution multiplier





## 遗留问题

- **结构问题：**结构简单，直筒结构，性价比不高，后续一系列的ResNet, DenseNet等结构已经证明通过复用图像特征， 使用concat/eltwise+ 等操作进行融合， 能极大提升网络的性价比。 

- **DWS问题：**Depthwise 部分的kernel比较容易训废掉,训出来的kernel有不少是空的(Relu)





## 补充

### n. 参数量和计算量、FLOPs

> 参数量是参与计算参数的个数,占用内存空间

若输入通道![C_{in}](https://math.jianshu.com/math?formula=C_%7Bin%7D)和输出通道![C_{out}](https://math.jianshu.com/math?formula=C_%7Bout%7D)，则参数量为：<a href="https://www.codecogs.com/eqnedit.php?latex=C_{in}*(K*K)&plus;1)*C_{out}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?C_{in}*(K*K)&plus;1)*C_{out}" title="C_{in}*(K*K)+1)*C_{out}" /></a>

> 计算量(乘加次数)，MAC(Multiply Accumulate)，需要考虑输出map的大小，1个MAC算两次操作

输入通道![C_{in}](https://math.jianshu.com/math?formula=C_%7Bin%7D)和输出通道![C_{out}](https://math.jianshu.com/math?formula=C_%7Bout%7D)，则计算量为：<a href="https://www.codecogs.com/eqnedit.php?latex=C_{in}*K*K*H_{out}*W_{out}*C_{out}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?C_{in}*K*K*H_{out}*W_{out}*C_{out}" title="C_{in}*K*K*H_{out}*W_{out}*C_{out}" /></a>

> 浮点运算量，指计算量（floating point operations）

若考虑偏置：FLOPs= <a href="https://www.codecogs.com/eqnedit.php?latex=(C_{in}*2*K*K)*H_{out}*W_{out}*C_{out}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?(C_{in}*2*K*K)*H_{out}*W_{out}*C_{out}" title="(C_{in}*2*K*K)*H_{out}*W_{out}*C_{out}" /></a>

不考虑偏置：FLOPs= <a href="https://www.codecogs.com/eqnedit.php?latex=(C_{in}*2*K*K-1)*H_{out}*W_{out}*C_{out}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?(C_{in}*2*K*K-1)*H_{out}*W_{out}*C_{out}" title="(C_{in}*2*K*K-1)*H_{out}*W_{out}*C_{out}" /></a>

