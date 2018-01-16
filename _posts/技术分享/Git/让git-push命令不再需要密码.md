---
title: 让git push命令不再需要密码
categories:
- 技术分享
- Git
date: 2017-02-09 04:13:11
tags: [Git,GitHub]
thumbnail: https://lh3.googleusercontent.com/-nnwlkrohkHo/WJvpWp8lRGI/AAAAAAAABcw/bl95WI6WWwI/s0/2017-02-09_13-00-25.png
---
<!--excerpt-->

最近利用jekyll写博客，为的就是博客管理方便，但是在上传博客的时候使用``git push``命令每次都得输入github帐号和密码特别的不方便，于是就搜了一下。

在这篇文章里提到，``GitHub``获得远程库时，有``ssh``方式和``https``方式。

![](https://lh3.googleusercontent.com/-ZUfeXHqFx4w/WJvtMYHET_I/AAAAAAAABdE/bJQdF5oIxHo/s0/2017-02-09_13-16-51.png)
![](https://lh3.googleusercontent.com/-sz3NFWCpl1E/WJvtWKfPzYI/AAAAAAAABdI/AmCzzhSMpJc/s0/2017-02-09_13-17-30.png)

两个方式的``url``地址不同，认证方式也不同。使用``ssh``时保存密钥对以后可以不再输入帐号密码，而``https``却不能。所以如果想要不再输入帐号密码，一种方式就是在``git clone``的时候使用``ssh``方式，另一种方式就是去修改已有项目``.git``目录下的``config``文件中的``url``，如下：

```
[remote "origin"]
    url = git@github.com:suyan/suyan.github.io.git
    fetch = +refs/heads/*:refs/remotes/origin/*
```