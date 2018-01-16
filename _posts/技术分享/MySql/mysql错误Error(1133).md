---
title: mysql错误Error(1133)
date: 2017-07-29 19:44:49
tags: [MySql]
keywords: [MySql]
categories: 
- 技术分享
- MySql
thumbnail: https://lh3.googleusercontent.com/-Nn6PgzF17Ls/WX6RRjefnYI/AAAAAAAADIU/Z6yZQdREfYgewhfbPXWf3PtGjTod9CD-ACHMYCw/s0/2017-07-31_11-09-08.png
---
<!--excerpt-->

### mysql错误Error(1133): Can’t find any matching row in the user table

刚尝试修改mysql数据库用户名时，提示"Error (1133): Can’t find any matching row in the user table"，遂找了下网上的解决办法：

从mysql错误代码说明里，可以查到mysql错误1133是数据库用户名不存在，如下：

```
1133：MYSQL数据库用户不存在
```

如此，找到的最简单的办法就是在mysql命令行中执行``FLUSH PRIVILEGES;``这一语句即可。

mysql提示1133错误的原因是，变更了mysql.user表之后，没有使用FLUSH PRIVILEGES命令来更新权限表（grant tables）。



