---
title: centos6 yum方式升级内核
date: 2017-07-31 06:28:41
tags: 
- docker
- centos
categories: 
- 技术分享
- Docker
thumbnail: https://lh3.googleusercontent.com/-HdgXZcekhys/WX6Wg2TmE7I/AAAAAAAADIs/_cRgLneca0U-IF6YnB0HrcqaLro55XITACHMYCw/s0/2017-07-31_11-31-30.png
---

最近没有时间好久没有写文章了，今天由于需要安装docker学习虚拟容器的知识，需要升级OS的内核。目前我这边使用的OS是centos6.5，内核是2.6版本的，如下：

cat /etc/issue

uname -r

因为docker的使用需要3.0以上内核的支持，当然也是可以使用2.6的内核，当时可能会出现不可控制的问题，所以需要我们升级内核版本。

要升级内核OS到3.1以上，需要以下几个步骤。

# **一、安装elrepo的yum源**

升级内核需要使用elrepo的yum源，在安装yum源之前还需要我们导入elrepo的key，如下：

rpm –import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

To install ELRepo for RHEL-7, SL-7 or CentOS-7:

rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm

To make use of our mirror system, please also install yum-plugin-fastestmirror.

To install ELRepo for RHEL-6, SL-6 or CentOS-6:

rpm -Uvh http://www.elrepo.org/elrepo-release-6-8.el6.elrepo.noarch.rpm

To make use of our mirror system, please also install yum-plugin-fastestmirror.

elrepo的key安装完毕后，我们下面开始正式升级内核。

# **二、升级内核**

在yum的elrepo源中有ml和lt两种内核，其中ml(mainline)为最新版本的内核，lt为长期支持的内核。

如果要安装ml内核，使用如下命令：

yum –enablerepo=elrepo-kernel -y install kernel-ml

如果要安装lt内核，使用如下命令：

yum –enablerepo=elrepo-kernel -y install kernel-lt

在此我们安装的是lt内核，如下：

内核升级完毕后，不会立即生效，还需要我们修改grub.conf文件。

# **三、修改grub.conf文件**

内核升级完毕后，需要我们修改内核的启动顺序，默认启动的顺序应该为1,升级以后内核是往前面插入为0，如下：

vim /etc/grub.conf

default=0

# **四、重启系统并查看系统内核**

grub.conf文件修改完毕后，还需要重启系统，如下：

shutdown -r now

系统启动完毕后，我们来查看内核版本，如下：

uname -r

通过上图，我们可以很容易的看出centos6.5已经升级内核到3.10版本。