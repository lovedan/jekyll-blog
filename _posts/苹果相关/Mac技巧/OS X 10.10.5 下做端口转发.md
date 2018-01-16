---
title: OS X 10.10.5 下做端口转发
categories:
- 苹果相关
- Mac技巧
date: 2017-02-11 18:30:08
tags: [Mac技巧]
thumbnail: https://lh3.googleusercontent.com/-ArCd3HrqaPI/WJ9aHU8B0vI/AAAAAAAABiE/6mVQaN5q50Q/s0/2017-02-12_03-38-19.png
---
<!--excerpt-->

网上搜索了一下相关的资料，都是介绍用pf来做转发，而且都是转发到127.0.0.1。由于我是需要转发到虚拟机上，按网上的教程测试了一下pf，结果是没能转发成功。最后找到了socat这个程序，终于解决问题。

安装socat，前提是你已经有homebrew，没有的话参考这篇来安装homebrew。
```
brew install socat
```
安装完成后，执行下面命令就可以完成转发。
```
sudo socat TCP4-LISTEN:80,fork TCP4:192.168.173.129:80
```
这条命令是将本机的80端口转发至虚拟机的80端口，虚拟机IP为192.168.173.129。

高级例子

```
lsof -c socat | grep LISTEN
socat -d -d -lf /var/log/socat.log TCP4-LISTEN:13389,reuseaddr,fork,su=nobody TCP4:192.168.1.209:3389 &
socat -d -d -lf /var/log/socat.log TCP4-LISTEN:5902,reuseaddr,fork,su=nobody TCP4:192.168.1.13:5900 &
lsof -c socat | egrep 'LISTEN|UDP'
socat UDP4-RECVFROM:500,fork UDP4-SENDTO:192.168.1.209:500
socat UDP4-RECVFROM:4500,fork UDP4-SENDTO:192.168.1.209:4500
lsof -i udp | egrep 'isakmp|ipsec-msft' #500，4500端口占用
```

操作完成。