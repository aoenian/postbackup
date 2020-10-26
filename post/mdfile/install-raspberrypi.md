---
title: 树莓派Raspbian系统安装过程
date: 2019-03-24
categories: Raspberry Pi
tags:
- ssh
- stretch
---

周末了，发现树莓派的pi-hole出问题了，无法提供dns服务了，没有查原因，想着用很长时间了，这次正好重新装一次系统，把之前不用的功能和软件全部处理掉。

英文不错请查看官方的[安装指南](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)。

<!--more-->

# Raspbian系统安装

## 系统下载

### 下载地址

这次不装桌面了，基本用不到，占资源，在[这里](https://www.raspberrypi.org/downloads/raspbian/)下载Lite系统，建议使用种子下载，很快。

### 校验镜像

因为在Windows上面做的镜像，所以这里的校验软件推荐Hasher，下载地址请点击[这里](http://www.den4b.com/products/hasher)。这个软件能够生成的校验码如下：**CRC32, MD2, MD4, MD5, SHA1, SHA256, SHA512, RipeMD128, RipeMD160, ED2K**。

## 系统安装

很久没装系统了，现在发现官方还是很贴心的，直接把烧录镜像的软件也推荐了，名字是[Etcher](https://www.balena.io/etcher/)，页面中间根据自己的操作系统选择下载，因为这种软件用的频率不高，这里建议大家就不要安装了，选择**Portable**。

接着就没啥好说的，下载的系统是压缩包，打开Etcher选择压缩包（不用解压），选择U盘（软件会自动识别），然后点击**Flash**就可以了。

系统完成后注意，**先不要插入树莓派启动，因为新版的系统默认没有开启ssh，系统烧录完成，请在系统中`boot`目录下新建一个名字为`ssh`的空文件（没有后缀）打开ssh。**

## 系统登录和基本配置

### 系统登录

在同一局域网的电脑上用ssh登录到树莓派系统，至于IP可以在路由器上查看。新系统的用户名为`pi` 密码为 `raspberry`。[官方文档](https://www.raspberrypi.org/documentation/faqs/#troubleshoot)

### 基本配置

登入系统后，执行如下命令

```shell
# 启动树莓派设置程序 也可用 sudo -i 切换为 root
sudo raspi-config  
# 更改密码
选择 change User Password
# 扩展系统空间
选择 Advanced Options  Expand Filesystem
```

这里说一下固定IP的配置，在网络找了很多方法，没有成功，最后我用路由器给树莓派分配了固定IP.

### 遇到的问题

在配置的时候会出现如下问题：

```shell
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
    LANGUAGE = (unset),
    LC_ALL = (unset),
    LC_CTYPE = "zh_CN.UTF-8",
    LANG = "en_GB.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("en_GB.UTF-8").
```

解决方法如下：

执行 `sudo raspi-config` 命令，然后选择 Localisation Options - change locale - 勾选 en_US.UTF-8  zh_CN.UTF-8 取消 en_GB.UTF-8 下一步 Default locale for the system environment 选择 en_US.UTF-8 确定