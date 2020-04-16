---
layout:     post
title:      “Support Vector Machine"
date:       2020-02-07 18:00:00
author:     "Tp"
header-img: "img/the_most_beautiful_road_in_the_world_2-wallpaper-1920x600.jpg"
tags:
    - MachineLearning
    - SVM
---

[toc]



SVM是一类按<u>监督学习</u>方式对数据进行非概率二元分类的广义线性分类器，学习策略是间隔最大化，使最终转化为凸二次规划问题求解

由简至繁有如下几个模型：

- 线性可分SVM：

  > 当训练数据线性可分时，通过硬间隔(==hard margin==)最大化可以学习得到一个线性分类器，即硬间隔SVM。

- 线性SVM：

  > 当训练数据不能线性可分但是可以近似线性可分时，通过软间隔(soft margin)最大化也可以学习到一个线性分类器，即软间隔SVM。

- 非线性SVM：

  > 当训练数据线性不可分时，通过使用核技巧(kernel trick)和软间隔最大化，可以学习到一个非线性SVM。



## 函数间隔与几何间隔

**函数间隔：** $\widehatγ = y(w T x + b) = yf(x)$,数据集中所有样本点的函数间隔最小值即为数据集的函数间隔

考虑到上述定义中若成比例修改$w$和$b$的值，$f(x)$的值也会改变，但是超平面并没有变化，因此要对$w$加上约束条件

**几何间隔：**t通过几何距离得出， $γ = \frac{(w^T x + b)}{ ∥w∥} = \frac{f(x)}{ ∥w∥}$,为了得到绝对值再乘上标签$y$,即$\widetilde γ =\frac{\widehatγ}{ ∥w∥}$，就是函数间隔除以$||w||$

因此要找的最大间隔分类器的目标函数可以定义为 $max \widetilde γ$，且根据间隔定义有：

​                                                       $y_i(w^T x_i + b) = \widehatγ_i ≥ \widehatγ, i = 1, ..., n$

假设 $\widehatγ = 1$，则目标函数转化成了：

​                                                        $$max \frac{1}{||w||},s.t. \ \  y_i(w^T x_i + b)\geq 1$$

## 深入推导

继续变换上述目标函数为：$min  \frac{1}{2} ||w||^2, \ \ s.t. \ \  y_i(w^T x_i + b)\geq 1$

由于目标函数是二次的，约束条件是线性的，问题最终被转化为一个凸二次规划问题，可直接使用 $Quadratic\ Programming$优化包求解



## 参考

1. 支持向量机通俗理论
2. [凸优化](/_posts/Machine Learning/2019-09-22-Convex-Optimization.md)

