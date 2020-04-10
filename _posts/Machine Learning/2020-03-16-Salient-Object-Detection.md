---
 layout:     post
title:      “Salient Object Detection"
date:       2020-02-07 18:00:00
author:     "Tp"
header-img: "img/the_most_beautiful_road_in_the_world_2-wallpaper-1920x600.jpg"
tags:
	- Machine Learning
---

> 基于FT显著性检测，SLIC超像素算法，Grabcut分割算法的代码实现

## FT显著性检测

主要利用的是颜色特征以及亮度特征：

- 对image进行高斯滤波得到gfrgb
- 将image图像由RGB颜色空间转换为LAB颜色空间
- 对转换后的图像L，A，B三个通道取均值
- 对LAB均值图像和滤波图像取欧式距离并求和
- 利用最大值对显著图归一化

```python
def FT(src):
    lab = cv2.cvtColor(src,cv2.COLOR_BGR2LAB)
    gaussian_blur=cv2.GaussianBlur(src,(5,5),0)

    mean_lab = np.mean(lab,axis=(0,1))
    
    salient_map = (gaussian_blur - mean_lab)*(gaussian_blur - mean_lab)
    salient_map = (salient_map-np.amin(salient_map))/(np.amax(salient_map)-np.amin(salient_map))
    
    return salient_map
```

   

## SLIC超像素分割算法



**Simple linear Iterative clustering:**是指具有相似纹理、颜色、亮度等特征的相邻像素构成的有一定视觉意义的不规则像素块。它利用像素之间特征的相似性将像素分组,用少量的超像素代替大量的像素来表达图片特征,很大程度上降低了图像后处理的复杂度，所以通常作为分割算法的预处理步骤。已经广泛用于图像分割、姿势估计、目标跟踪、目标识别等计算机视觉应用。 将彩色图像转化为CIELAB颜色空间和XY坐标下的5维特征向量，然后对5维特征向量构造距离度量标准，对图像像素进行局部聚类的过程。

- 将图像由RGB转为LAB空间，因为LAB的颜色更丰富
- 定义超像素生成的数量K，即一个M✖️N的图像每个像素块有（M✖️N）/K个像素，可视作每个超像素边长为`S=sqrt((M✖️N)/K)`
- 取（S/2，S/2）为像素中心点，后续将会以中心点进行聚类操作，而由于这个中心点可能会落在噪声点或者像素边缘，利用差分方法进行梯度计算，调整中心点
- 以2S✖️2S的区域进行聚类

```python
import math
from skimage import io, color
import numpy as np
from tqdm import trange
import time

class Cluster(object):
    cluster_index = 1

    def __init__(self, h, w, l=0, a=0, b=0):
        self.update(h, w, l, a, b)
        self.pixels = []
        self.no = self.cluster_index
        Cluster.cluster_index += 1

    def update(self, h, w, l, a, b):
        self.h = h
        self.w = w
        self.l = l
        self.a = a
        self.b = b

    def __str__(self):
        return "{},{}:{} {} {} ".format(self.h, self.w, self.l, self.a, self.b)

    def __repr__(self):
        return self.__str__()


class SLICProcessor(object):
    @staticmethod
    def open_image(path):
        """
        Return:
            3D array, row col [LAB]
        """
        rgb = io.imread(path)
        lab_arr = color.rgb2lab(rgb)
        return lab_arr

    @staticmethod
    def save_lab_image(path, lab_arr):
        """
        Convert the array to RBG, then save the image
        :param path:
        :param lab_arr:
        :return:
        """
        rgb_arr = color.lab2rgb(lab_arr)
        io.imsave(path, rgb_arr)

    def make_cluster(self, h, w):
        h = int(h)
        w = int(w)
        return Cluster(h, w,
                       self.data[h][w][0],
                       self.data[h][w][1],
                       self.data[h][w][2])

    def __init__(self, filename, K, M):
        self.K = K
        self.M = M

        self.data = self.open_image(filename)
        self.image_height = self.data.shape[0]
        self.image_width = self.data.shape[1]
        self.N = self.image_height * self.image_width
        self.S = int(math.sqrt(self.N / self.K))

        self.clusters = []
        self.label = {}
        self.dis = np.full((self.image_height, self.image_width), np.inf)

    def init_clusters(self):
        h = self.S / 2
        w = self.S / 2
        while h < self.image_height:
            while w < self.image_width:
                self.clusters.append(self.make_cluster(h, w))
                w += self.S
            w = self.S / 2
            h += self.S

    def get_gradient(self, h, w):
        if w + 1 >= self.image_width:
            w = self.image_width - 2
        if h + 1 >= self.image_height:
            h = self.image_height - 2

        gradient = self.data[h + 1][w + 1][0] - self.data[h][w][0] + \
                   self.data[h + 1][w + 1][1] - self.data[h][w][1] + \
                   self.data[h + 1][w + 1][2] - self.data[h][w][2]
        return gradient

    def move_clusters(self):
        for cluster in self.clusters:
            cluster_gradient = self.get_gradient(cluster.h, cluster.w)
            for dh in range(-1, 2):
                for dw in range(-1, 2):
                    _h = cluster.h + dh
                    _w = cluster.w + dw
                    new_gradient = self.get_gradient(_h, _w)
                    if new_gradient < cluster_gradient:
                        cluster.update(_h, _w, self.data[_h][_w][0], self.data[_h][_w][1], self.data[_h][_w][2])
                        cluster_gradient = new_gradient

    def assignment(self):
        for cluster in self.clusters:
            for h in range(cluster.h - 2 * self.S, cluster.h + 2 * self.S):
                if h < 0 or h >= self.image_height: continue
                for w in range(cluster.w - 2 * self.S, cluster.w + 2 * self.S):
                    if w < 0 or w >= self.image_width: continue
                    L, A, B = self.data[h][w]
                    Dc = math.sqrt(
                        math.pow(L - cluster.l, 2) +
                        math.pow(A - cluster.a, 2) +
                        math.pow(B - cluster.b, 2))
                    Ds = math.sqrt(
                        math.pow(h - cluster.h, 2) +
                        math.pow(w - cluster.w, 2))
                    D = math.sqrt(math.pow(Dc / self.M, 2) + math.pow(Ds / self.S, 2))
                    if D < self.dis[h][w]:
                        if (h, w) not in self.label:
                            self.label[(h, w)] = cluster
                            cluster.pixels.append((h, w))
                        else:
                            self.label[(h, w)].pixels.remove((h, w))
                            self.label[(h, w)] = cluster
                            cluster.pixels.append((h, w))
                        self.dis[h][w] = D

    def update_cluster(self):
        for cluster in self.clusters:
            sum_h = sum_w = number = 0
            for p in cluster.pixels:
                sum_h += p[0]
                sum_w += p[1]
                number += 1
            _h = int(sum_h / number)
            _w = int(sum_w / number)
            cluster.update(_h, _w, self.data[_h][_w][0], self.data[_h][_w][1], self.data[_h][_w][2])

    def save_current_image(self, name):
        image_arr = np.copy(self.data)
        for cluster in self.clusters:
            for p in cluster.pixels:
                image_arr[p[0]][p[1]][0] = cluster.l
                image_arr[p[0]][p[1]][1] = cluster.a
                image_arr[p[0]][p[1]][2] = cluster.b
            image_arr[cluster.h][cluster.w][0] = 0
            image_arr[cluster.h][cluster.w][1] = 0
            image_arr[cluster.h][cluster.w][2] = 0
        self.save_lab_image(name, image_arr)

    def iterate_10times(self):
        self.init_clusters()
        self.move_clusters()
        for i in trange(10):
            self.assignment()
            self.update_cluster()
            name = 'image_M{m}_K{k}_loop{loop}.jpg'.format(loop=i, m=self.M, k=self.K)
            self.save_current_image(name)


if __name__ == '__main__':
    start = time.time()
    p = SLICProcessor('1.jpg', 1000, 40)
    p.iterate_10times()
    end = time.time()
    print('Slic用时：',end-start)
```



## Grabcut分割算法

利用了图像中的纹理（颜色）信息和边界（反差）信息，只需要在目标外面画一个框，把目标框住，它就可以完成良好的分割



## Reference

1. [Frequency-tuned Salient Region Detection](/paper/Salient Object Detection/Frequency-tuned Salient Region Detection.pdf)
2. [SLIC Superpixel](/paper/Salient Object Detection/SLIC Superpixels.pdf)

