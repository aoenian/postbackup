---
title: macOS通过FTP管理小米手机文件
date: 2019-05-12
categories: macOS
tags:
- miui
- ftp
- Android
---

手机和电脑之间的文件传输一直感觉比较麻烦，很多时候我用的是树莓派作为家庭的局域网的文件中转，然后用ES文件管理器。后来ES更新以后感觉不好用就没再安装。而前面把树莓派给重装了以后还没有配置共享。

今天需要处理手机里面的文件，突然想起小米手机自带的文件管理器有一个ftp功能。这次试了一下，感觉还不错，简单的管理都是可以满足的，也不用额外安装手机软件。

<!--more-->

具体的使用如下：

## 手机里面的文件下载

1. 手机端启动ftp服务

    - 保证手机和电脑在同一个wifi下

    - 打开手机的**文件管理应用**-最上面切换到**分类**-最后一项**远程管理**-启动服务

    - 手机端可看到让你在**我的电脑**的地址栏输入一个ftp地址如：<ftp://192.168.*.*:2121>

2. 电脑端访问

    - Windows请通过我的电脑的地址栏访问上一步中出现的地址，即可进行文件管理（_下面的不用看了_）

    - macOS建议浏览器（极大的避免乱码的出现，macOS用finder会出现乱码）

## 把电脑的文件上传到手机（macOS）

macOS的finder以及浏览器只能查看和下载手机中的文件，不能进一步管理如删除、新建等操作。而且新版的系统也把`ftp`命令删除掉了。

- ftp软件方法

    这里推荐两个个人用过的：[FileZilla](https://filezilla-project.org/download.php?type=client) [cyberduck](https://cyberduck.io/) 第一个是开源软件，第二个是官网下载免费。

    如何连接不多说了，只是注意一点**编码使用GBK**。

- 命令行方法

    这个没有试过，想在命令行使用ftp命令需要安装**inetutils**，命令如下`brew install inetutils`。

参考博文：

- [MacOS High Sierra kills terminal FTP?](https://discussions.apple.com/thread/8093031)

- [无线FTP里的中文乱码](http://www.miui.com/thread-1137434-1-1.html)

- [OS X 下，最理想的 FTP 工具是什么？](https://www.zhihu.com/question/20107568)