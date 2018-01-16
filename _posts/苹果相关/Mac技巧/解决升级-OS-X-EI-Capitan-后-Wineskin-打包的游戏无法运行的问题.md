---
title: 解决升级 OS X EI Capitan 后 Wineskin 打包的游戏无法运行的问题
tags: [Mac技巧]
categories: 
- 苹果相关
- Mac技巧
date: 2017-01-20 04:05:05
thumbnail: 
---
<!--excerpt-->

此前苹果发布了 OS X 的新版本 EI Capitan，系统有更新当然是立马进行升级享受新鲜事物带来的快感了，但前几天想玩耍一局 Pro Evolution Soccer(实况足球)的时候，发现我深爱的游戏再也启动不起来了，和同样爱玩儿实况的同事交流之后发现情况大致有如下两种：

1. 双击 ``.app`` 文件，程序是可以打开的，但是窗口始终无法显示（我的电脑）；
2. 双击 ``.app`` 文件，程序无法打开，系统提示 X11 无法打开，截图如下（同事的）：

![](https://lh3.googleusercontent.com/-QBgvDnnM1I8/WIF8A7QmcZI/AAAAAAAAAx4/9lYwjuvkybE/2017-01-20_11-54-59.png)

无法在绿茵场上尽情驰骋，这种事显然不能忍，立马去 [Wineskin 官网](http://wineskin.urgesoftware.com/)看了看，发现有一条关于 OS X EI Capitan 的新闻：

> Wineskin does not currently work correctly on El Capitan. The issue is being looked into, but I have no ETA for a fix. I'll get a fix out as soon as I can.
> Some people have been able to upgrade to the latest version of XQuartz, and change their wrappers to use XQuartz and not WineskinX11 and have gotten wrappers to work, but this method has not worked for everyone.
> Please add to the discussion here if you are helping find a solution.

又看到有一条新版本发布的新闻：

> I've released an update for Wineskin... version 2.6.1.
> I think I've fixed all the El Capitan bugs and it should work fine on 10.6 - 10.11 now.
> important point. This has to change how an engine is installed in a wrapper slightly. If you update a wrapper to 2.6.1 and it does not work right, please reinstall the engine in the wrapper. You can just use Change Engine and install the same one back in, but it will install it correctly for 2.6.1 and then things should work right. Newly made wrappers should have no issues.

这说明官方发布的最新的 2.6.1 版本已经解决了该问题，那么剩下的问题就简单多了，我们只需要将 `Wineskin Wrapper` 给替换成官方发布的最新版本即可。

## 解决办法

1. 在应用程序中找到需要修复的游戏，右键选择 `显示包内容`；
2. 打开 `Wineskin.app`，选择 `advance`，记录下引擎版本号：

![](https://lh3.googleusercontent.com/-6iUQqxE4ehs/WIGN2RoMBgI/AAAAAAAAAyY/PZ-MapzWiSU/s0/2017-01-20_13-11-04.png)

3. 下载最新版本的 [Wineskin Winery](http://wineskin.urgesoftware.com/tiki-index.php?page=Downloads)；
4. 打开 `Wineskin Winery`，添加一个新的 `engine`，版本号最好与之前记录的版本号一致：

![](https://lh3.googleusercontent.com/-6GZhT7VGSoE/WIGOD7jQBmI/AAAAAAAAAyc/8Vsd_dVjhUA/s0/2017-01-20_13-11-58.png)

5. 更新 `Wrapper Version` 至最新版，只要在 2.6.1 以上即代表已经使用了官方修复 OS X EI Capitan 问题的 `Wrapper`：

![](https://lh3.googleusercontent.com/-YqFjJh_LWPA/WIGOQFuz0TI/AAAAAAAAAyg/N5YYm23sKAc/s0/2017-01-20_13-12-47.png)

6. 选择 `Create New Blank Wrapper`，填写游戏名称并选择 `Create`。如果有提示需要安装东西，都可以选择 `Cancel`；
7. 创建成功后，将原游戏 `.app` 文件包内容中的 `Content/Resources/driver_c` 文件夹以及 `Content/Resources/system.reg`、`Content/Resources/user.reg`、`Content/Resources/userdef.reg` 这三个文件覆盖到我们新创建的 `Wrapper` 文件包内容中的 `Content/Resources/` 文件夹中，它们分别相当于 `Windows` 系统中的 C 盘和注册表；
8. 再打开我们新创建的 `Wrapper` 文件包内容中的 `'Wineskin.app'`，选择 `advance`，然后选择程序的启动路径，即刚刚覆盖的 `driver_c` 文件夹中我们想要启动的 `exe` 程序：

![](https://lh3.googleusercontent.com/-D0qlGz7IKW4/WIGOfMRqr1I/AAAAAAAAAyk/rRYqm5pR1LA/s0/2017-01-20_13-13-48.png)

9. 退回到我们在第 6 步中生成新 Wineskin Wrapper 的目录，当然现在它已经是一个完整的游戏程序了，双击运行它，然后 enjoy it!

鉴于评论区有朋友反映下载 `engine` 和 `wrapper` 总是失败，所以现在将最新的 `engine` 和 `wrapper` 上传到网盘供大家下载：

10. Engine 1.7.52 [下载地址](http://pan.baidu.com/s/1bnoelND)，提取码：`ikhq`；
11. Wrapper 2.6.1 [下载地址](http://pan.baidu.com/s/1c0c6Qo4)，提取码：`v1dh`。

下载后，将 `WS9Wine1.7.52.tar.7z` 这个文件放入 `~/Library/Application Support/Wineskin/Engines` 这个文件夹；将 `Wineskin-2.6.1.app.zip` 这个文件解压缩，得到 `Wineskin-2.6.1.app` 这个文件，将其放入 `~/Library/Application Support/Wineskin/Wrapper` 这个文件夹，然后重新打开上面第 3 步中下载的 `Wineskin Winery`，如果 `engine` 和 `wrapper` 都已经存在了，直接进行第 6 步的操作即可。

-------------------------------------------
再放上 `Wineskin Winery` 的网盘地址：

Wineskin Winery 1.7 [下载地址](http://pan.baidu.com/s/1hq8TO8G)，提取码：`dpdu`。