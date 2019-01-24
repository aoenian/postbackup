---
title: VirtualBox虚拟系统使用共享文件夹和摄像头
date: 2019-01-08
categories: macOS
tags:
- 虚拟机
- 增强功能
---

macOS用着挺好，不过Windows依然离不了，所以虚拟机还得用。为啥不用双系统？切换太麻烦。而且浪费空间，本来硬盘就不大。

虚拟机如果没有特殊要求，个人推荐[VirtualBox](https://www.virtualbox.org/)，安装包个头小，而且免费使用，稳定性也不错。

具体的安装过程就不说了，你都用上虚拟机了，这点问题应该能解决，给大家一个下载系统的网站 [I Tell You](https://msdn.itellyou.cn/)。

这里要说的就是如何扩展虚拟机的功能实现与主机文件共享并在虚拟机中使用摄像头。
<!-- more -->

# 环境

- macOS 10.13.6

- VirtualBox 5.2.6

- 虚拟系统 Windows7 专业版

# 操作方法

主要就是安装增强功能。

1. 完成虚拟系统的安装后启动Windows系统，然后在VirtualBox的菜单栏单击**设备**-**增强功能**。

    然后Windows系统就会弹出**VirtualBox Guest Additions**的光盘自动运行界面，然后点击运行**VBoxWindowsAdditions.exe**即可。

2. 安装完成后关闭虚拟系统，选中虚拟系统右键**设置**，选择**共享文件夹**。

    然后添加共享文件夹（_右边有一个绿色的加号的文件夹图标_）。选择路径（_注意这个共享目录是mac中的_），键入共享名称，勾选**自动挂载**。这里注意共享文件夹是固定分配。

3. 完成后启动Windows，在**资源管理器中**—**计算机**下即可看到共享目录（_以网络驱动器形式展现_）。

4. 如果你没有在Windows中使用摄像头的需求就可以结束了。如果需要则继续操作。

    在[virtualbox](https://download.java.net/virtualbox)下载与你的虚拟机版本相同的`extpack`来使用mac的摄像头。

    如我的VirtualBox版本为5.2.6，则下载`Oracle_VM_VirtualBox_Extension_Pack-5.2.6-120293.vbox-extpack`。

5. 下载完成后安装，然后打开虚拟系统，点击VirtualBox软件菜单中的**设备**即可看到摄像头（Webcams）。

注意：`VBoxGuestAdditions_5.2.6.iso`也可下载后安装。下载地址同[extpack](https://download.java.net/virtualbox)地址。

参考：

- [Virtualbox 虚拟机支持硬件摄像头](http://www.54php.cn/default/178.html)

- [VboxDownload](https://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html)
