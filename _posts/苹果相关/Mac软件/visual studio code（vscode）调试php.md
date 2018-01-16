---
title: visual studio code（vscode）调试php
date: 2017-09-16 10:48:27
tags: [vscode,Mac软件]
categories: 
- 苹果相关
- Mac软件
thumbnail: https://lh3.googleusercontent.com/-nV1uTHeFNGA/Wa4KdnKpzOI/AAAAAAAADc4/gLSx2GPOV_8DxfrlZdX7ROvrvSB7RjY1gCHMYCw/I/15045781654402.jpg
---
<!--excerpt-->

1.下载vscode ([visual studio code](https://www.visualstudio.com/))。

2.安装vscode 扩展 php-debug 安装步骤见 https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug。

3.安装wampserver,我安装的是2.5 版本，安装步骤自行百度。

4.确保apache的80 端口未占用，如果占用了修改httpd.conf 配置文件下的端口号

5.修改php.ini（wamp\bin\apache\apache2.4.9\bin 这个文件夹下的） 文件开启debug，

修改以下两项：

xdebug.remote_enable = on

xdebug.remote_autostart=on

6.如果vscode 报这个![](https://lh3.googleusercontent.com/-Esav1O51PeE/Wa36WFVWIUI/AAAAAAAADcM/OnkMHQkNFog_LndPMSF-W5LT2ly5XPpmgCHMYCw/I/284201-20160314155819521-1359852711.png)

修改用户配置![](https://lh3.googleusercontent.com/-f7ZC2KSNy9U/Wa36WQQ6wLI/AAAAAAAADcQ/Us2JKD5ToP0o8DDxISl6NluvI9sNeAr0gCHMYCw/I/284201-20160314155836381-404314791.png)

![](https://lh3.googleusercontent.com/-g9cD6Pdp-W8/Wa36WlS1upI/AAAAAAAADcU/ghJhtkYU_okjLT7beWDDcWgQReGvh67qACHMYCw/I/284201-20160314160038474-30091765.png)

7.配置debug

![](https://lh3.googleusercontent.com/-yH-r7YX1MYo/Wa36W9scymI/AAAAAAAADcY/Kv4FT96AQlI0yz-UH2gFP9EGEbmOeb4bwCHMYCw/I/284201-20160314160221240-268065764.png)

选择listen for xdebug

![](https://lh3.googleusercontent.com/-qKWaE8AZqN8/Wa36XMSuMaI/AAAAAAAADcc/hJ_406dfl1EgZu9FfzZdk95y_sYvmzSiwCHMYCw/I/284201-20160314160322303-2072419759.png)

8.启动wampserver

9.在需要的地方打上断点 F5 启动，在浏览器里输入地址，vscode 自动会停在断点处

![](https://lh3.googleusercontent.com/-C5V_QFRHE68/Wa36Xuv0acI/AAAAAAAADcg/WgB_JEfyzRsIfLVr1chqfK7cVGAur9ttwCHMYCw/I/284201-20160314160930990-363539015.png)



