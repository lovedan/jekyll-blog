---
title: Laravel5导入自定义函数和类的方法
date: 2016-12-01 07:19:55
tags: [laravel]
keywords: [laravel]
categories: 
- 技术分享
- Laravel
thumbnail: https://dl.dropboxusercontent.com/s/onmlew6rmnd33ct/SuRyW14362693901Laravel-5%5B1%5D.png?dl=0
---

我们的应用里经常会有一些全局都可能会用的函数，我们应该怎么放置它会比较好呢？以下有一种推荐的方式：
<!--excerpt-->
## 导入自定义函数

我们的应用里经常会有一些全局都可能会用的函数，我们应该怎么放置它会比较好呢？以下有一种推荐的方式：

1. 创建文件 ``app/helpers.php``

```
<?php

// 示例函数
function foo() {
    return "foo";
}
```

2. 修改项目 ``composer.json``

在项目 composer.json 中 autoload 部分里的 files 字段加入该文件即可：

```
{
    ...

    "autoload": {
        "files": [
            "app/helpers.php"
        ]
    }
    ...
}
```
然后运行:
```
composer dump-autoload
```

OK，然后你就可以在任何地方用到 ``app/helpers.php`` 中的函数了。

## 导入自定义类

比如说我要加载一个名字是BaiduPCS.php类文件

修改项目 ``composer.json``

在项目 composer.json 中 autoload 部分里的 classmap 字段加入该文件所在的文件夹即可：

```
    "autoload": {
        "classmap": [
            "database",
            "app/DDL/Classes"  <== 就是这里
        ],
        "files": [
            "app/DDL/Functions/functions.php"
        ],
        "psr-4": {
            "App\\": "app/",
            "DDL\\":"app/DDL/"
        }
    },
```
然后运行:
```
composer dump-autoload
```

OK，然后你就可以在你要使用该类的地方添加以下代码来使用。

```
use BaiduPCS;
```