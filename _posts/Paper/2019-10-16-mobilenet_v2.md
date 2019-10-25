---
layout:     post
title:      “MobileNets_v2"
date:       2019-10-16 08:00:00
author:     "Tp"
header-img: "/img/the_most_beautiful_road_in_the_world_2-wallpaper-1920x1080.jpg"
tags:
    - CV 
    - Paper
---

# MobileNetV2: Inverted Residuals and Linear Bottlenecks

> based on an inverted residual structure where the shortcut connections are between the thin bottleneck layers.
>
> 主要解决了V1在训练过程中非常容易特征退化的问题

## Depthwise Separable Convolutions

略



## Inverted residuals

常规的`residuals block`是首先使用一个`1✖️1`的卷积降低通道数，再通过卷积，最后重新用一个`1✖️1`的卷积将通道数扩张回去，此处倒置过来的意义在于DWConv本来就采集的特征很少了，所以，先使用`1✖️1`的卷积扩张通道，再进行DWConv，最后再把通道降回来。

![](/img/mobilenet/残差与倒置残差.png)





## Linear Bottlenecks

> ReLU会对channel数较低的张量造成较大的信息损耗。如下图所示，当原始输入维度数增加到15以后再加ReLU，基本不会丢失太多的信息；但如果只把原始输入维度增加至2~5后再加ReLU，则会出现较为严重的信息丢失，除非`imput manifold`可以嵌入到激活空间的显著的低维子空间。

![](/img/mobilenet/relu不同维度表现.png)

因此执行完降维的卷积后不会再使用ReLU





## 补充

### 1.[Manifold(流形)](https://www.cnblogs.com/jiangxinyang/p/9314256.html)



