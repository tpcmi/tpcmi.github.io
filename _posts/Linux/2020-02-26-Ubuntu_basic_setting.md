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

## 4. 终端设置分辨率

```
xrandr    //查看当前的输出设备以及有哪些分辨率
xrandr --output 设备名 --mode 分辨率
```

## 5. 查看端口是否被占用

```cmd
netstat -anp |grep Num
or
lsof -i: Num
```



## 6. 创建新用户、群组

root下创建在home下有完整用户名文件的用户

``` 
sudo adduser username
```

home文件夹下即有新用户名文件，但是初始创建时只会有`example.desktop`

`````linux
xgd-user-dirs-update
`````

生成所有一系列的文件如documents、Music ....etc.

在每次重启系统时，可以选择哪个用户进行登录，此后再使用`who`指令就是系统启动时的用户，不管是否有切换为其他用户

---
创建用户组

```
groupadd -g gid groupname
```

添加或删除成员

```lilnux
gpasswd -a 成员名 gpname
gpasswd -d 成员名 gpname
```

查看群组成员

````linux
cat(vim) /etc/group
````

修改文件所属群组

````linux
chgrp [option] gpname 文件名
````

修改文件所属者

```linux
chown [option] gpname 文件名
```

修改文件权限

r：4

w：2

x：1

满权限值为7，三个位置设置即可

```linux
chmod 777 文件名 //各个级别权限多少由位置上的数值决定
```

- root不论有没有较高的权限，都可以读写执行

- 群组组员继承群组的权限，若所有者也属于群组，所有者的权限不受群组影响

  

## 7. Anaconda 多用户共用

虽然anaconda明面上安装在某个子用户的文件下，但是通过查看`ls -al`可知安装anaconda时权限属于root组，所以可以直接在新的用户`.bashrc`文件中加入anaconda的路径：

````linux
export PATH=/home/username/anaconda3/bin:$PATH
````

以上即可以在新的用户里使用conda命令

## 8. 复制、移动、删除

```linux
cp [option] 源文件 目标文件	// 一般 -r 目录复制就可以了，-p：将所有属性权限也复制
rm [option] 目标文件	//-f：强制不会有任何报错；
mv [option] 源文件 目标文件	//可以顺便改名
```

## 9. 图形界面关闭开启



````linux
service lightdm stop/start    //ubuntu
startx    //centos
````

重新开启图形界面时，如果出现`lightdm.service masked`

```linux
systemctl unmask lightdm.service
```

再重新安装一次lightdm即可

```linux
sudo apt install lightdm
```

再重新打开即可

## 10. 虚拟机联网

````llinux
sudo service network-manager stop
sudo rm /var/lib/NetworkManager/NetworkManager.state
sudo service network-manager start
````

