---
layout:     post
title:      FAS论文阅读（二）
subtitle:   Face Liveness Detection using Local Diffused Patterns
date:       2020-02-02 17:00:00
author:     "Tp"
header-img: "/img/the_most_beautiful_road_in_the_world_2-wallpaper-1920x1080.jpg"
tags:
    - CV 
    - Paper
    - FAS
---

[toc]

## Summary

本文提出一种基于`diffussion speed`的方法进行活体检测， 该方法的核心思想在于活体与攻击对象的光照漫反射速度不同

> The whole idea was based on the fact that the illumination energies diffuse slowly on a uniform 2D surface, whereas these energies move faster on a 3D live face because of its nonuniformity.

![](/img/FAS/Local Diffused pattern/QQ20200203-195309.png)

**diffused speed：**原图与`diffused picture`在每个像素点上的差值。

以下为简易流程图：
![](/img/FAS/Local Diffused pattern/QQ20200203-200807.png)

> Firstly, diffused image is obtained by calculating the diffusion of the original image by applying AOS (Additive Operator Splitting) scheme with TDMA (TriDiagonal Matrix Algorithm) to solve the diffusion equation. Subsequently, delta image is obtained by finding the difference of the original image and diffused image and locally diffused patterns at each pixel position of the delta image are considered as the baseline feature. Extracted features are fed to Support Vector Machine to classify whether input image is live image or fake image.
>
> 首先，原始图像通过采用AOS（Additive Operator Splitting）算法和TDMA (TriDiagonal Matrix Algorithm)算法来获得`diffused image`。然后通过原始图像与`diffused image`之间的差分得到`delta image`，并将`delta image `每个像素点局部漫射模式作为特征图的基线。提取特征喂入SVM中去分类输入的图像是否为活体图像或是假的图像。



### 计算 _Diffused Image_

































## 补充

[^1]: 

## References

1. [Face Liveness Detection From a Single Image via Diffusion Speed Model](/paper/FAS local diffused pattern/Face Liveness Detection From a Single Image via Diffusion Speed Model.pdf)
2. 