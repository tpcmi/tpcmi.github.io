---
layout:     post
title:      “Function of Evaluation"
date:       2019-09-20 15:00:00
author:     "Tp"
header-img: "img/the_most_beautiful_road_in_the_world_2-wallpaper-1920x600.jpg"
tags:
    - DeepLearning
---

机器学习(ML), 自然语言处理(NLP), 信息检索(IR)等领域, 评估(Evaluation)是一个必要的工作, 而其评价指标往往有如下几点: 准确率(Accuracy), 精确率(Precision), 召回率(Recall) 和 F1-Score.

通过具体数据进行分析更容易理解，假设现有一网络用来活体检测，数据集中真人脸假人脸各有50张，经过网络预测后得出，真人脸mis-classified有15张，假人脸mis-classified有10张。

> 此处引入几个概念：
>
> **---> *true positives:***  **TP**  正类判定为正类  ✔️
>
> **--->*false positives:***  **FP**  负类判定为正类  ❌
>
> **--->*false negatives:***  **FN**  正类判定为负类  ❌
>
> **--->*true negatives:***  **TN**  负类判定为负类  ✔️

此刻 `TP=35`,`FP=10`,`FN=15`,`TN=40`

- **精确率（precision）**:
$P=\dfrac {TP}{TP+FP}$
- **召回率（recall）：**
$R=\dfrac {TP}{TP+FN}$
- **F1**的值就是精确率和召回率的调和均值：
$\dfrac {2}{F_{1}}=\dfrac {1}{p}+\dfrac {1}{R}$

调整后

$F_{1}=\dfrac {2PR}{P+R}=\dfrac {2TP}{2TP+FP+FN}$



