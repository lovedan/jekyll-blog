---
title: 开启Fusion Drive功能教程 双硬盘 SSD+HDD
tags: [Mac技巧]
categories: 
- 苹果相关
- Mac技巧
date: 2017-02-03 05:44:35
thumbnail: https://www.dropbox.com/s/m7m65qw1u7dre5g/2017-02-08_10-23-21.png?dl=1
---
<!--excerpt-->

废话不多说进入主题

![](https://lh3.googleusercontent.com/-E0utAkBrw68/WJp0KJxr95I/AAAAAAAABYg/fg4_1VUTdq0/s0/2017-02-08_10-28-03.png)

> disk0 是用的 120g 三星ssd 固态硬盘
> disk1 是用的 250g 机器自带的普通硬盘

![](https://lh3.googleusercontent.com/-R8GL5Cm9cMc/WJp0abZJ9MI/AAAAAAAABYk/oRlTT6-3-yQ/s0/2017-02-08_10-29-08.png)

1.开启Fusion Drive功能之前，先要把数据全部备份。

![](https://lh3.googleusercontent.com/-22yQinjGFrQ/WJp0snqYIqI/AAAAAAAABYs/8LvjpzO5bgQ/s0/2017-02-08_10-30-22.png)

2.用系统启动光盘或者u盘启动电脑。

![](https://lh3.googleusercontent.com/-7RdUviFxN98/WJp04GLOjZI/AAAAAAAABYw/6KVCkKBO5FI/s0/2017-02-08_10-31-07.png)

3.进入磁盘工具把2块硬盘都进行格式化

![](https://lh3.googleusercontent.com/-TEUn4kf4Z50/WJp1ZegbCuI/AAAAAAAABY4/menESopf7v0/s0/2017-02-08_10-33-20.png)

4.退出磁盘工具，进入终端。

输入: 
```
diskutil list
```
查看当前磁盘的详细信息

![](https://lh3.googleusercontent.com/-TM1DhU3tVjc/WJp2R9Uy2ZI/AAAAAAAABZE/5r0reM3fbEQ/s0/2017-02-08_10-37-06.png)

5.合并磁盘

输入:
```
diskutil cs create bla disk0 disk1
```

![](https://lh3.googleusercontent.com/-em2gG4BcWQA/WJp2lN41i5I/AAAAAAAABZI/DDjUR0LyBHQ/s0/2017-02-08_10-38-23.png)

这里可以看见最后一行 百分百的进度条。。。

![](https://lh3.googleusercontent.com/-maQp1HMUN9A/WJqhrcq7ZFI/AAAAAAAABZk/Eqk6lvsq83Q/s0/2017-02-08_13-42-15.png)

画横线的地方显示正在创建逻辑盘。。。

![](https://lh3.googleusercontent.com/-Flp1ZQVqu6Q/WJqhzS5hOcI/AAAAAAAABZo/aKnVGV3ZxnE/s0/2017-02-08_13-42-48.png)

这里显示逻辑盘已经创建成功

最后一行显示

Core Storage LVG UUID: ``【BBC6AD77-ADF2-40EE-A1D0-68BFE4913966】``

Finished CoreStorage operation

``【】``中的字串之后还会用到，可以直接复制使用

6.再次输入:
```
diskutil cs list
```
查看逻辑磁盘状态

![](https://lh3.googleusercontent.com/-55OmC1i9yOM/WJqiNghRqXI/AAAAAAAABZs/mxIFLTqrwSQ/s0/2017-02-08_13-44-32.png)

显示新的逻辑卷的容量和``id``，下面有组成逻辑卷的硬盘``id``
秘书用的是``120+250``的硬盘，容量是``377.4G``，实际可用``375.2G``

7.建立Fusion Drive

输入:    
```
diskutil coreStorage createVolume BBC6AD77-ADF2-40EE-A1D0-68BFE4913966 jhfs+ blub 375g
diskutil coreStorage createVolume 【BBC6AD77-ADF2-40EE-A1D0-68BFE4913966】 jhfs+ blub 375g
```
其中``【】``里面的id换成之前查到的自己的逻辑卷id，``【】``不要需要输入进去的。

建立的容量不能大于实际可用容量

8.退出终端 进入磁盘工具，看下是否已经成功开启Fusion Drive。

![](https://lh3.googleusercontent.com/-V4J2mg_cGaY/WJqjk4iTFvI/AAAAAAAABZ8/Fj_iYgqMVvY/s0/2017-02-08_13-50-22.png)

9.接下来点 重新安装系统，后面就和普通安装没有任何区别了。

![](https://lh3.googleusercontent.com/-Wbn3v2OHp_g/WJqjsnZ754I/AAAAAAAABaA/CQdziT0dQSo/s0/2017-02-08_13-50-54.png)
![](https://lh3.googleusercontent.com/--cDPwpcrjdU/WJqj9ZiIjbI/AAAAAAAABaE/p3zyHD91k6k/s0/2017-02-08_13-52-01.png)
![](https://lh3.googleusercontent.com/-m8qhXLJ-RoU/WJqkHAkXXmI/AAAAAAAABaM/Lp6Q0_zhXqk/s0/2017-02-08_13-52-39.png)