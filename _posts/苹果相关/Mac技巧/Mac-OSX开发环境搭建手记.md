---
title: Mac OSX开发环境搭建手记
categories:
- 苹果相关
- Mac技巧
date: 2017-03-23 04:47:40
tags: [Mac技巧,nginx,php]
thumbnail: https://2.bp.blogspot.com/-OKiVYaYS_lc/WXxj-Hs1M7I/AAAAAAAADGc/bOqdRU_Ur80C9wP5DAtWg3juAuRKrRNPQCKgBGAs/s1600/2017-02-16_10-38-03.png
---
<!--excerpt-->

## Brew
> Brew 是 Mac 下面的包管理工具，通过 Github 托管适合 Mac 的编译配置以及 Patch，可以方便的安装开发工具。 Mac 自带ruby 所以安装起来很方便，同时它也会自动把git也给你装上。官方网站： http://brew.sh 。

> 安装完成之后，建议执行一下自检，brew doctor如果看到Your system is ready to brew. 那么你的brew已经可以开始使用了。

### 安装：
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
### 自检：
```
brew doctor
```
### 常用命令：（所有软件以php7.1为例子）
```
brew update                        #更新brew可安装包，建议每次执行一下
brew search php71                  #搜索php7.1
brew tap josegonzalez/php          #安装扩展<gihhub_user/repo>   
brew tap                           #查看安装的扩展列表
brew install php71                 #安装php7.1
brew remove  php71                 #卸载php7.1
brew upgrade php71                 #升级php7.1
brew options php71                 #查看php7.1安装选项
brew info    php71                 #查看php7.1相关信息
brew home    php71                 #访问php7.1官方网站
brew services list                 #查看系统通过 brew 安装的服务
brew services cleanup              #清除已卸载无用的启动配置文件
brew services restart php71        #重启php-fp
```

注意：brew services 相关命令最好别经常用了，提示会被移除

```
➜  ~  brew services restart php71
Warning: brew services is unsupported and will be removed soon.
You should use launchctl instead.
Please feel free volunteer to support it in a tap.

Stopping `php71`... (might take a while)
==> Successfully stopped `php71` (label: homebrew.mxcl.php71)
==> Successfully started `php71` (label: homebrew.mxcl.php71)
```

## Oh My Zsh
> ohmyzsh & iTerm2两个神器，在Mac os x下是一定要装的. 两组配合起来使用，加上插件。简直是神一样的存在。 秒杀梅西，内马尔啊：） Oh 猛戳到官网

### 安装 oh my zsh
```
curl -L http://install.ohmyz.sh | sh
```
## 安装终端的命令自动提示

在终端下操作会经常需要输入一些常用的命令，要不出错的输入这些命令也不是件容易的事，zsh还有一个很好用的补全命令的插件zsh-autosuggestions, 我们通过下面命令安装下：

[官方地址](https://github.com/zsh-users/zsh-autosuggestions)
```
Oh My Zsh

1. Clone this repository into $ZSH_CUSTOM/plugins (by default ~/.oh-my-zsh/custom/plugins)

2 .git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
Add the plugin to the list of plugins for Oh My Zsh to load:

3. plugins=(zsh-autosuggestions)

4. Start a new terminal session. 
```

好了，然后你可以执行source ~/.zshrc或者重新打开终端，看下效果：

![自动提示](https://lh3.googleusercontent.com/-kmKtrstTeOI/WNSJJjAa2BI/AAAAAAAAB3E/1fbBeha76NM/s0/2017-03-24_11-49-10.png)

如上图，我输入一个b就会自动提示我之前输入过的命令，然后按一下右方向键就能自动补全了。
### 设置默认shell

> 查看系统支持的shell列表，Mac 10.9.4 自带了 zsh 5.0.2，Linux上得安装。

```
cat /etc/shells
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
zsh --version
zsh 5.0.2 (x86_64-apple-darwin13.0)
chsh -s /bin/zsh
```

> 虽然Mac自带了zsh，如果你想要最新版的zsh，那么你用 brew install zsh安装一个最新的吧。/usr/local/bin/zsh --version zsh 5.0.5 (x86_64-apple-darwin13.3.0) 区别也不会很大， 默认的版本已经很新了。

# MySQL PHP Nginx
## 安装MySQL
```
brew install mysql
```
MySQL开机启动：

```
mkdir LaunchAgents
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```

安装完成之后开启MySQL安全机制：

```
/usr/local/opt/mysql/bin/mysql_secure_installation
```

根据终端提示，输入root密码，然后依次确认一些安全选项。具体信息可以参考[外国友人的这篇文章](http://blog.frd.mn/install-nginx-php-fpm-mysql-and-phpmyadmin-on-os-x-mavericks-using-homebrew/)

## 安装PHP
添加brew的PHP扩展库：

```
brew update
brew tap homebrew/dupes
brew tap josegonzalez/homebrew-php
```

可以使用`brew options php71`命令来查看安装php7.1的选项，这里我用下面的选项安装：

```
brew install php70 --with-debug --with-gmp --with-homebrew-curl --with-homebrew-libressl --with-homebrew-libxml2 --with-homebrew-libxslt --with-imap --with-libmysql
```

> PHP编译过程中如果遇到`configure: error: Cannot find OpenSSL's <evp.h>`错误，执行`xcode-select --install`重新安装一下**Xcode Command Line Tools** 在[GitHub HomeBrew](https://github.com/Homebrew/homebrew-php/issues/1181)上有关于这个讨论:
> *For future reference of anybody looking for Command Line Tools with Xcode 5, open up a Terminal window and type xcode-select --install. A window will appear informing you command line tools are required. Click Install and you should be good to go*

> 我安装的时候还遇到了一个奇怪的问题，提示fatal error: 'my_global.h' file not found #include <my_global.h> 就是提示各种`.h`的文件不存在。各种google以后发现了一个论坛的资料给了我一些启发，[搓这里](https://www.blitzbasic.com/Community/posts.php?topic=102579)

里面提到了一下内容
```
/usr/local/mysql/include
```

先删除`/usr/local/include`下的mysql文件夹然后执行下面命令

```
ln -sfv /usr/local/Cellar/mysql/5.7.17/include/mysql/* /usr/local/include
```

等待PHP编译完成，开始安装PHP常用扩展，扩展安装过程中brew会自动安装依赖包，例如`php71-pdo-pgsql` 会自动装上`postgresql`,这里我安装以下PHP扩展：

```
brew install php71-apcu\
 php71-gearman\
 php71-geoip\
 php71-gmagick\
 php71-imagick\
 php71-intl\
 php71-mcrypt\
 php71-memcache\
 php71-memcached\
 php71-mongo\
 php71-opcache\
 php71-pdo-pgsql\
 php71-phalcon\
 php71-redis\
 php71-sphinx\
 php71-swoole\
 php71-uuid\
 php71-xdebug;

```

> 扩展里面提一下[php71-phalcon](http://phalconphp.com/) 和 [php71-swoole](http://www.swoole.com/). 一个是C语言写的PHP框架，安装来个人摸索熟悉一下，还没有真正的使用过，大致看了一下文档，感觉非常吊炸天。目前公司的项目是基于Yii2的，也看看这个框架。 另外一个[swoole是国产的PHP高性能网络通信框架](http://www.swoole.com/)，貌似不错，可能在项目中会考虑用到它。

由于Mac自带了php和php-fpm，因此需要添加系统环境变量PATH来替代自带PHP版本,我们用的是zsh,所以放进.zshrc中,如果你用的shell是bash,那么可以把下面的信息写入到~/.bash_profile文件中，如果这个文件没有，你自己建一个就行。

```
echo 'export PATH="$(brew --prefix php70)/bin:$PATH"  #for php' >> ~/.zshrc
echo 'export PATH="$(brew --prefix php70)/sbin:$PATH"  #for php-fpm' >> ~/.zshrc
echo 'export PATH="/usr/local/bin:/usr/local/sbib:$PATH" #for other brew install soft' >> ~/.zshrc
source ~/.zshrc
```

```
echo 'export PATH="$(brew --prefix php71)/bin:$PATH"' >> ~/.bash_profile  #for php
echo 'export PATH="$(brew --prefix php71)/sbin:$PATH"' >> ~/.bash_profile  #for php-fpm
echo 'export PATH="/usr/local/bin:/usr/local/sbib:$PATH"' >> ~/.bash_profile #for other brew install soft
source ~/.bash_profile

```

测试一下效果：

```
#brew安装的php 他在/usr/local/opt/php71/bin/php
php -v    
PHP 7.1.3 (cli) (built: Jul 16 2014 15:43:06) (DEBUG)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
    with Zend OPcache v7.0.3, Copyright (c) 1999-2014, by Zend Technologies
    with Xdebug v2.2.5, Copyright (c) 2002-2014, by Derick Rethans 

#Mac自带的PHP
/usr/bin/php -v   
PHP 5.4.24 (cli) (built: Jan 19 2014 21:32:15) 
Copyright (c) 1997-2013 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies

#brew安装的php-fpm 他在/usr/local/opt/php71/sbin/php-fpm
php-fpm -v
PHP 7.1.3 (fpm-fcgi) (built: Jul 16 2014 15:43:12) (DEBUG)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
    with Zend OPcache v7.0.3, Copyright (c) 1999-2014, by Zend Technologies
    with Xdebug v2.2.5, Copyright (c) 2002-2014, by Derick Rethans

#Mac自带的php-fpm
/usr/sbin/php-fpm -v
PHP 5.4.24 (fpm-fcgi) (built: Jan 19 2014 21:32:57)
Copyright (c) 1997-2013 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies
```

修改php-fpm配置文件，`vim /usr/local/etc/php/7.0/php-fpm.conf`，找到pid相关大概在25行，去掉注释 `pid = run/php-fpm.pid`, 那么php-fpm的pid文件就会自动产生在`/usr/local/var/run/php-fpm.pid`，下面要安装的Nginx pid文件也放在这里。


```
#测试php-fpm配置
php-fpm -t
php-fpm -c /usr/local/etc/php/7.0/php.ini -y /usr/local/etc/php/7.0/php-fpm.conf -t

#启动php-fpm
php-fpm -D
php-fpm -c /usr/local/etc/php/7.0/php.ini -y /usr/local/etc/php/7.0/php-fpm.conf -D

#关闭php-fpm
kill -INT `cat /usr/local/var/run/php-fpm.pid`

#重启php-fpm
kill -USR2 `cat /usr/local/var/run/php-fpm.pid`

#也可以用上文提到的brew命令来重启php-fpm，不过他官方不推荐用这个命令了
brew services restart php71

#还可以用这个命令来启动php-fpm

launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.php70.plist
```

启动php-fpm之后，确保它正常运行监听9000端口：

```
lsof -Pni4 | grep LISTEN | grep php
php-fpm   30907 calvin    9u  IPv4 0xf11f9e8e8033a2a7      0t0  TCP 127.0.0.1:9000 (LISTEN)
php-fpm   30917 calvin    0u  IPv4 0xf11f9e8e8033a2a7      0t0  TCP 127.0.0.1:9000 (LISTEN)
php-fpm   30918 calvin    0u  IPv4 0xf11f9e8e8033a2a7      0t0  TCP 127.0.0.1:9000 (LISTEN)
php-fpm   30919 calvin    0u  IPv4 0xf11f9e8e8033a2a7      0t0  TCP 127.0.0.1:9000 (LISTEN)
#正常情况，会看到上面这些进程
```

PHP-FPM开机启动：

```
ln -sfv /usr/local/opt/php70/*.plist ~/Library/LaunchAgents
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.php70.plist
```

补充

```
php-fpm的一些管理:

#测试php-fpm配置
php-fpm -t

#启动php-fpm
php-fpm -D

#关闭php-fpm
kill -INT `cat /usr/local/var/run/php-fpm.pid`

#重启php-fpm
kill -USR2 `cat /usr/local/var/run/php-fpm.pid`

#也可以用上文提到的brew命令来管理php-fpm
brew services start|stop|restart php70

#还可以用这个命令来管理php-fpm
php70-fpm start|stop|restart 
```

以上安装出现的问题记录：

1. 使用Curl出现的502错误#

做有赞接口的时候发现出现了502错误，用下面的测试代码也可以测试：
```
   $curl = curl_init(); 
        // 抓取支付宝首页测试 
        curl_setopt($curl, CURLOPT_URL, 'https://www.alipay.com'); 
        // 设置header 
        curl_setopt($curl, CURLOPT_HEADER, 1); 
        // 设置cURL 参数，要求结果保存到字符串中还是输出到屏幕上 
        curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1); 
        // 运行cURL，请求网页数据 
        $data = curl_exec($curl); 
        // 关闭cURL请求 
        curl_close($curl); 
        // 打印出抓取的测试数据 
        var_dump($data);
```
产生问题的原因：brew安装curl时默认没有带上--with-openssl
解决方法：

```
# 先删除curl
brew uninstall curl

# 重新安装curl,带上--with-openssl
brew install curl --with-openssl

# 或者使用brew reinstall
brew reinstall curl --with-openssl
```

然后重启下php-fpm

**安装php composer**

```
brew install composer
#检查一下情况
composer --version
Composer version 1.0.0-alpha8 2014-01-06 18:39:59

```

> redis memcached这些软件brew 已经自动依赖安装上，如果想开机自动启动，或者查看使用说明 `brew info redis`即可。另外，composer的中文文档：[猛戳这里](http://composer.golaravel.com/)

## 安装Nginx

首先查看一下可选的编译参数

brew options nginx
输出结果如下：

```
--with-debug
        Compile with support for debug log
--with-gunzip
        Compile with support for gunzip module
--with-passenger
        Compile with support for Phusion Passenger module
--with-spdy
        Compile with support for SPDY module
--with-webdav
        Compile with support for WebDAV module
--devel
        install development version 1.7.7
--HEAD
        install HEAD version
```

可以看到可选的编译参数里没有 GeoIP 模块，那么如果还想用 Homebrew 来安装 nginx，就只有自己修改默认的 Formula 来添加参数，或者使用其他人修改好后共享的 Formula 了。

考虑到之后可能还会用到其他模块，我就没有自己创建 Formula，而是找到了 homebrew/nginx 这个 tap。

首先给 Homebrew 添加这个 tap：

```
brew tap homebrew/nginx
```
再查看一下可选参数：

```
brew options nginx-full
```
可以在结果中看到有 WebDav 模块：

```
--with-dav-ext-module
	Build with HTTP WebDav Extended support
```
这样就可以安装有 WebDav 模块的 nginx 了：

```
brew install nginx-full --with-webdav --with-dav-ext-module
```
不只是 WebDav 模块，其他可选模块也可以用同样方法添加.

nginx启动关闭命令：

```
测试配置是否有语法错误
nginx -t
```

#打开 nginx
```
sudo nginx
```
#重新加载配置|重启|停止|退出 nginx
```
nginx -s reload|reopen|stop|quit
```
#也可以使用Mac的launchctl来启动|停止
```
launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
```

Nginx开机启动

```
ln -sfv /usr/local/opt/nginx-full/*.plist ~/Library/LaunchAgents
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.nginx-full.plist
```
Nginx监听80端口需要root权限执行，因此：

```
sudo chown root:wheel /usr/local/opt/nginx-full/bin/nginx
sudo chmod u+s /usr/local/opt/nginx-full/bin/nginx
```

配置nginx.conf
创建需要用到的目录：
```
mkdir -p /usr/local/var/log/nginx
mkdir -p /usr/local/etc/nginx/sites-available
mkdir -p /usr/local/etc/nginx/sites-enabled
mkdir -p /usr/local/etc/nginx/conf.d
mkdir -p /usr/local/etc/nginx/ssl
```

vim /usr/local/etc/nginx/nginx.conf 输入以下内容：

```
worker_processes  1;

error_log   /usr/local/var/log/nginx/error.log debug;


pid        /usr/local/var/run/nginx.pid;


events {
    worker_connections  256;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /usr/local/var/log/access.log  main;

    sendfile        on;
    keepalive_timeout  65;
    port_in_redirect off;

    include /usr/local/etc/nginx/sites-enabled/*;
}
```
设置nginx php-fpm配置文件

```
vim /usr/local/etc/nginx/conf.d/php-fpm
#proxy the php scripts to php-fpm
location ~ \.php$ {
    try_files                   $uri = 404;
    fastcgi_pass                127.0.0.1:9000;
    fastcgi_index               index.php;
    fastcgi_intercept_errors    on;
    include /usr/local/etc/nginx/fastcgi.conf;
}
```

创建默认虚拟主机default
vim /usr/local/etc/nginx/sites-available/default输入：

```
server {
    listen       80;
    server_name  localhost;
    root         /var/www/;

    access_log  /usr/local/var/logs/nginx/default.access.log  main;

    location / {
        index  index.html index.htm index.php;
        autoindex   on;
        include     /usr/local/etc/nginx/conf.d/php-fpm;
    }

    location = /info {
        allow   127.0.0.1;
        deny    all;
        rewrite (.*) /.info.php;
    }

    error_page  404     /404.html;
    error_page  403     /403.html;
}
```

创建ssl默认虚拟主机default-ssl
vim /usr/local/etc/nginx/sites-available/default-ssl输入：

```
server {
    listen       443;
    server_name  localhost;
    root       /var/www/;

    access_log  /usr/local/var/logs/nginx/default-ssl.access.log  main;

    ssl                  on;
    ssl_certificate      ssl/localhost.crt;
    ssl_certificate_key  ssl/localhost.key;

    ssl_session_timeout  5m;

    ssl_protocols  SSLv2 SSLv3 TLSv1;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers   on;

    location / {
        include   /usr/local/etc/nginx/conf.d/php-fpm;
    }

    location = /info {
        allow   127.0.0.1;
        deny    all;
        rewrite (.*) /.info.php;
    }

    error_page  404     /404.html;
    error_page  403     /403.html;
}

```

创建phpmyadmin虚拟主机

vim /usr/local/etc/nginx/sites-available/phpmyadmin #输入以下配置

```
server {
    listen       306;
    server_name  localhost;
    root    /usr/local/share/phpmyadmin;

    error_log   /usr/local/var/logs/nginx/phpmyadmin.error.log;
    access_log  /usr/local/var/logs/nginx/phpmyadmin.access.log main;

    ssl                  on;
    ssl_certificate      ssl/phpmyadmin.crt;
    ssl_certificate_key  ssl/phpmyadmin.key;

    ssl_session_timeout  5m;

    ssl_protocols  SSLv2 SSLv3 TLSv1;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers   on;

    location / {
        index  index.html index.htm index.php;
        include   /usr/local/etc/nginx/conf.d/php-fpm;
    }
}
```

设置SSL

```
mkdir -p /usr/local/etc/nginx/ssl
openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=US/ST=State/L=Town/O=Office/CN=localhost" -keyout /usr/local/etc/nginx/ssl/localhost.key -out /usr/local/etc/nginx/ssl/localhost.crt
openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=US/ST=State/L=Town/O=Office/CN=phpmyadmin" -keyout /usr/local/etc/nginx/ssl/phpmyadmin.key -out /usr/local/etc/nginx/ssl/phpmyadmin.crt
```

创建虚拟主机软连接，开启虚拟主机

```
ln -sfv /usr/local/etc/nginx/sites-available/default /usr/local/etc/nginx/sites-enabled/default
ln -sfv /usr/local/etc/nginx/sites-available/default-ssl /usr/local/etc/nginx/sites-enabled/default-ssl
ln -sfv /usr/local/etc/nginx/sites-available/phpmyadmin /usr/local/etc/nginx/sites-enabled/phpmyadmin
```

启动|停止Nginx

```
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.nginx-full.plist
launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.nginx-full.plist
```

设置快捷服务控制命令
为了后面管理方便，将命令 alias 下，vim ~/.zsh_aliases 输入一下内容：

```
alias nginx.start='launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.nginx-full.plist'
alias nginx.stop='launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.nginx-full.plist'
alias nginx.restart='nginx.stop && nginx.start'
alias php-fpm.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.php70.plist"
alias php-fpm.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.php70.plist"
alias php-fpm.restart='php-fpm.stop && php-fpm.start'
alias mysql.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist"
alias mysql.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist"
alias mysql.restart='mysql.stop && mysql.start'
alias redis.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist"
alias redis.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist"
alias redis.restart='redis.stop && redis.start'
alias memcached.start="launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.memcached.plist"
alias memcached.stop="launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.memcached.plist"
alias memcached.restart='memcached.stop && memcached.start'
```

```
#让快捷命令生效
echo "[[ -f ~/.zsh_aliases ]] && . ~/.zsh_aliases" >> ~/.zshrc     
source ~/.zshrc
#创建站点目录到主目录，方便快捷访问
ln -sfv /var/www ~/htdocs
```
