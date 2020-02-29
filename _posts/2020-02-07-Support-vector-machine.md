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

SVM是一类按<u>监督学习</u>方式对数据进行非概率二元分类的广义线性分类器。

由简至繁有如下几个模型：

- 线性可分SVM：

  > 当训练数据线性可分时，通过硬间隔(hard margin，什么是硬、软间隔下面会讲)最大化可以学习得到一个线性分类器，即硬间隔SVM。

- 线性SVM：

  > 当训练数据不能线性可分但是可以近似线性可分时，通过软间隔(soft margin)最大化也可以学习到一个线性分类器，即软间隔SVM。

- 非线性SVM：

  > 当训练数据线性不可分时，通过使用核技巧(kernel trick)和软间隔最大化，可以学习到一个非线性SVM。

