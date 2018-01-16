---
title: 找回Sierra允许“任何来源”的应用
tags: [Mac技巧]
categories: 
- 苹果相关
- Mac技巧
date: 2017-02-16 16:13:07
thumbnail: https://lh3.googleusercontent.com/-L_GN75b_Z1g/WKXR1WbJc-I/AAAAAAAABpc/AbhsDUtKmzo/s0/2017-02-17_01-22-44.png
---
<!--excerpt-->

苹果秋季发布会之后，同时进行了两大平台系统的升级（Mac OS 和 iOS），这两天安装第三方Mac应用的时候，突然发现应用权限选项“任何来源”不翼而飞了

![](http://upload-images.jianshu.io/upload_images/355579-85be4234e1b9bf47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这就直接导致安装第三方app失败了，google了一下找到了找回“任何来源”方法。

打开终端，键入命令，输入密码，然后回车
```
sudo spctl --master-disable
```
然后打开“安全性与隐私”，发现久违的“任何来源”回来了

![](http://upload-images.jianshu.io/upload_images/355579-7ae01f6b94f2df4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 注：如果在系统偏好设置的“安全与隐私”中重新选中允许App Store 和被认可的开发者App，允许“任何来源”App的选项会再次消失，可运行上述命令再次打开“任何来源”。