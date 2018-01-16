---
title: MacOSX10.11开启任意显示器HiDPI方法
date: 2016-11-08 05:27:16
tags: [Mac技巧]
categories: 
- 苹果相关
- Mac技巧
thumbnail: https://lh3.googleusercontent.com/-WeKKYXOBCRE/WKUCgnwOJhI/AAAAAAAABoM/9DVKBjbXeR4/s0/2017-02-16_10-38-03.png
---
MacOSX10.11开启任意显示器HiDPI方法
<!--excerpt-->

>研究了下，其实没怎么变化。Overrides文件夹目录位置变了而已。
>本人开启HiDPI现在更主要是为了截图截出2x的效果，实际调整分辨率的是另一个小显示器~已经很久用不到HiDPI的真实意义啦


## 对于权限问题的补充
10.11系统权限设置又改动，一些系统文件只有“系统”有权限读写，首先要关闭这个权限：
开机按住 **command ＋ R**，进入恢复模式，然后在“终端”中输入“csrutil disable”关闭权限。如果需要打开，则csrutil enable。
这是在知乎上看到相同的问题，别人提到的。LZ好像没遇到过这种情况，如果有人遇到同样的权限问题，这样解决就好了。

### 对于权限问题的补充
1. 开启HiDPI
打开终端 键入
sudo defaults write /Library/Preferences/com.apple.windowserver DisplayResolutionEnabled -bool YES
回车后，输入当前系统管理员的密码，继续回车确认。

2. 获取你的显示器的两个 ID:
DisplayVendorID和DisplayProductID
打开终端, 命令分别是:
```
ioreg -l | grep "DisplayVendorID"
ioreg -l | grep "DisplayProductID"
```
> OK.在桌面上新建一个文件夹,名字格式是:DisplayVendorID-XXXX,其中XXXX是你的DisplayVendorID的16进制值小写.
于是,我会新建一个 DisplayVendorID-XXXX的文件夹,然后在这个文件夹里面新建一个空白文件.名字格式是
DisplayProductID-YYYY,自然YYYY就是你的DisplayProductID的16进制了.
我新建的文件是 DisplayProductID-YYYY.
最好下载我提供的模板编辑.
相信你不一定能找到个合适的进制转换工具，我从网上找到了一个很好用的flash，并把它放到了自己的服务器里，
大家如有需要随时可以去用
在线进制转换器
建议使用PlistPro工具编辑，方便快捷。.
**范例:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>DisplayProductID</key>
        <integer>10496</integer>
        <key>DisplayVendorID</key>
        <integer>8547</integer>
        <key>scale-resolutions</key>
        <array>
                <data>
                AAAKAAAABkAAAAABACAAAA==
                </data>
                <data>
                AAAUAAAADIAAAAABACAAAA==
                </data>
                <data>
                AAAKAAAABDgAAAABACAAAA==
                </data>
                <data>
                AAAUAAAACHAAAAABACAAAA==
                </data>
                <data>
                AAAHgAAABDgAAAABACAAAA==
                </data>
                <data>
                AAAPAAAACHAAAAABACAAAA==
                </data>
                <data>
                AAAGkAAABBoAAAABACAAAA==
                </data>
                <data>
                AAANIAAACDQAAAABACAAAA==
                </data>
                <data>
                AAAGQAAAA4QAAAABACAAAA==
                </data>
                <data>
                AAAMgAAABwgAAAABACAAAA==
                </data>
                <data>
                AAAFoAAAA4QAAAABACAAAA==
                </data>
                <data>
                AAALQAAABwgAAAABACAAAA==
                </data>
                <data>
                AAAINAAAA4QAAAABACAAAA==
                </data>
                <data>
                AAAQaAAABwgAAAABACAAAA==
                </data>
        </array>
</dict>
</plist>
```

建议下载本帖子中的附件。包含本范例。

最后面那一坨,和以及里面的data如何来的
比如我想使用1600900这个HiDPI,那么我就需要生成两个分辨率,其中一个是1600900,一个是其双倍,3200*1800.
1600,900两个值的16进制是00000640 00000384 ;
3200,1800两个值的16进制是00000C80 00000708;
后面加上 00000001 00200000
于是会得到
```
00000640 00000384 00000001 00200000
00000C80 00000708 00000001 00200000
```
用附件中的PlistPro编辑这个DisplayProductID-YYYY,计算并填写你想要的分辨率.
最后,把这个 DisplayVendorID-XXXX 文件夹,

 


拷贝到
```
/System/Library/Displays/Contents/Resources/Overrides/
```
(10.10及以下是 /System/Library/Displays/Overrides/ )
重启系统就可以看到了.可以安装RDM切换,在任务栏,方便快捷.
各位可以根据自己的屏幕规格来添加.

范例中，设定的分辨率是
2560x1600 2x (16:10)
2560x1080 2x (21:9)
1920x1080 2x (16:9)
1680x1050 2x (16:10)
1600x900 2x(16:9)
1440x900 2x(16:10)
2100x900 2x(21:9)