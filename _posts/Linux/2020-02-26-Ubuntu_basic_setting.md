---
layout:     post
title:      “Ubuntu 基础配置"
date:       2020-02-26 16:00:00
author:     "Tp"
header-img: "img/the_most_beautiful_road_in_the_world_2-wallpaper-1920x600.jpg"
tags:

    - Linux
---

## 虚拟机时间校准
```
rm /etc/localtime
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

