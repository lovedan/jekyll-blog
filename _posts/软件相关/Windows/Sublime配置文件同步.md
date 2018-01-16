---
title: Sublime配置文件同步
categories:
  - 软件相关
  - Windows
date: 2017-02-22 04:14:37
tags: [Sublime,编程软件]
thumbnail: https://lh3.googleusercontent.com/-Xc2Gu93qCgQ/WK0RD3uALXI/AAAAAAAABrc/gjwwbrMYRlE/s0/2017-02-22_13-18-23.png
---
<!--excerpt-->

# Sublime Config

The simplest method of installation is through the Sublime Text console. The console is accessed via the ctrl+` shortcut or the View > Show Console menu. Once open, paste the appropriate Python code for your version of Sublime Text into the console.

```
import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

## 账户 SSH 公钥
> 账户 SSH 公钥是跟用户账户关联的公钥，一旦设置，SSH 就拥有账户下所有项目仓库的读写权限。 设置“账户 SSH 公钥”是开发者使用 SSH 方式访问/修改代码仓库的“前置工作”，分为“获取 SSH 协议地址”、“生成公钥”、“在 Coding.net 添加公钥”三个步骤。

## 获取 SSH 协议地址
>在项目的代码页面点击 SSH 切换到 SSH 协议， 获得 clone 地址，形如git@git.coding.net:wzw/leave-a-message.git。 请使用这个地址来访问您的代码仓库。

## 生成公钥
> Mac/Linux 打开命令行终端, Windows 打开 Git Bash 。 输入ssh-keygen -t rsa -C “username@example.com”,( 注册的邮箱)，接下来点击enter键即可（也可以输入密码）。

```
$ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# Creates a new ssh key, using the provided email as a label
# Generating public/private rsa key pair.
Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]  // 推荐使用默认地址,如果使用非默认地址可能需要配置 .ssh/config
```

成功之后

```
Your identification has been saved in /Users/you/.ssh/id_rsa.
# Your public key has been saved in /Users/you/.ssh/id_rsa.pub.
# The key fingerprint is:
# 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```

## 在 Coding.net 添加公钥
>本地打开 id_rsa.pub 文件（或执行 $cat id_rsa.pub ），复制其中全部内容，添加到账户“SSH 公钥”页面 中，公钥名称可以随意起名字。
完成后点击“添加”，然后输入密码或动态码即可添加完成。 图片
完成后在命令行测试，首次建立链接会要求信任主机。

## Git 初始化

…or create a new repository on the command line
```
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:lovedan/test.git
git push -u origin master
```
…or push an existing repository from the command line
```
git remote add origin git@github.com:lovedan/test.git
git push -u origin master
```

1. git pull push在没有指定branch报错的解决方法
> 解决方案：指定当前工作目录工作分支，跟远程的仓库，分支之间的链接关系。比如我们设置master对应远程仓库的master分支git branch --set-upstream master origin/master这样在我们每次想push或者pull的时候，只需要 输入git push 或者git pull即可。

2. git pull 遇到fatal: refusing to merge unrelated histories
> git pull origin branchname --allow-unrelated-histories