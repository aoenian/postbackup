---
title: Fedora28配置记录1——基本配置
date: 2018-07-07
categories: Linux
tags:
- fedora
- xfce
---

从使用manjaro以后感觉对中文的支持不是太好以后，就想换一个发行版，然后想到了fedora。

之前虽然也用过但是那时候觉得不是太好，好像连exfat格式的u盘都无法识别。现在趁着这个机会再装一次看看。

安装系统
========

依然选择轻量级桌面，这次不再用lxde，选择了xfce，主要是相对kde和gnome要轻量，同时呢也有主题、图标的配置，终端的配色支持都有。比较折中。

[xfce镜像](https://spins.fedoraproject.org/xfce/)记得下载完成后验证镜像是否完整，写入u盘的软件我使用的是 [Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)

**建议安装的时候选择中文版，这样输入法中文字体都会有，可以安装完成后再修改为英文。**

<!--more-->

系统更新
========

安装rpmfusion-free
------------------

什么是[RPM Fusion](https://rpmfusion.org/RPM%2520Fusion)。配置介绍[Configuration](https://rpmfusion.org/Configuration/)

``` bash
# Fedora 22 and later 安装命令
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

更新系统
--------

``` bash
# 不用更换软件源，官方的就很快
# 切换root用户或使用sudo命令
dnf makecache
dnf update
```

**更新完成系统后能够直接打开 `exfat` 格式的u盘**

软件安装配置
============

安装
----

``` bash
# wine软件根据个人需要，如果你不用qq可以不安装
dnf install zsh git vim wine
```

oh-my-zsh
---------

项目地址：[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

安装方法：检查系统是否安装 `curl` 或 `wget` 命令，并选择相应的命令执行，如果两个命令都没有那就最好装一个。

``` bash
# wget
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
# curl
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

**安装完成后记得切换默认shell为zsh，执行命令 `chsh -s /bin/zsh` 需要注销才能起作用**

Tim或QQ
-------

我使用的是github上面的[Wine-QQ-TIM](https://github.com/askme765cs/Wine-QQ-TIM)项目。直接下载Appimage版本的TIM或者QQ。

下载地址：

> Wine-QQ:<http://yun.tzmm.com.cn/index.php/s/XRbfi6aOIjv5gwj>
> Wine-TIM:<http://yun.tzmm.com.cn/index.php/s/5hJNzt2VR9aIEF2>

**在manjaro、Fedora28 xfce、Ubuntu18.04中安装 `wine` 以后都能正常运行，不过可能会出现字体发虚，中文输入的问题。**

*在Fedora28 Xfce中测试后是可以接收文件、用ibus输入中文的* 这里我建议大家不要太纠结腾讯的软件问题，毕竟移动终端已经很方便了，一般用PC端的情况也不多，如果能够基本满足就不用总折腾这个东西，浪费时间。

浏览器安装
----------

Opear浏览器下载地址：[Opera browser for Linux](https://www.opera.com/computer/linux)下载相应的RPM包以后，直接双击安装即可。

chromium浏览器安装可以直接使用命令 `sudo dnf install chromium-xx.x86_64`

### Opera富强

**2018-07-08虽然今天看的时候富强功能已经和谐了，不过更改系统的语言地区的方法还是记录一下吧**

-   找到系统的语言设置，位置在 `Administration-Language`
-   更改里面的语言为其他地区的语言，可以选择 `zh_HK.utf8`
-   然后注销，回到登录界面
-   在登录界面右上角同样有一个语言选项，也更改为 `zh_HK.utf-8` **这个更改很重要不然不起作用**
-   修改完成后登入系统，这时候部分语言已经更改过来，建议重启完成完整的语言更换

**注意：如果大家使用英文的话，默认是无法输入中文，即使你启动ibus也不行，请使用下面的方法**

``` bash
# fedora默认使用 im-chooser 管理输入法
im-chooser
# 在弹出的界面中选择 ibus 即可输入中文
```
