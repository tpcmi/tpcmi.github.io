---
layout:     post
title:      “Ubuntu 基础配置"
date:       2020-02-26 16:00:00
author:     "Tp"
header-img: "img/the_most_beautiful_road_in_the_world_2-wallpaper-1920x600.jpg"
tags:

    - Linux
---

[toc]

## 1. 虚拟机时间校准

```
sudo rm /etc/localtime
sudo ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

## 2. 异常退出后 “cannot find a valid peer process to connect to”

主要解决方法是kill所有的VM的进程重新打开虚拟机

```
ps x|grep vmware    //关闭软件后，找出所有还在运行的VM进程
kill -9 PID_number
```

## 3. 使用 apt install 时“Could not get lock /var/lib/dpkg/lock” & “dpkg: error: dpkg frontend is locked by another process”

```
ps afx|grep apt    //找出所有apt进程
sudo kill -9 PID_number
then
sudo rm /var/lib/dpkg/lock-frontend
sudo apt-get install -f
```

