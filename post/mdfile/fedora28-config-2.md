---
title: Fedora28配置记录2——系统美化
date: 2018-07-14
categories: Linux
tags:
- fedora
- system beauty
---

接上篇，搞定输入法然后想把系统整得好看一点。

输入法安装
==========

如果对中文输入法需求不高，ibus的自带拼音输入也就够用了，不过这个经常打交道的家伙还是来个顺手的更好。这里依然推荐[Rime](https://rime.im/)中州韵输入法，安装很简单一条命令就可以处理，而且如果你用全拼的话基本不用配置即可使用，需要了解的是输入法的配置快捷键是`` CTRL+` ``

``` bash
sudo dnf install ibus-rime
```

<!--more-->

图标和主题配置
==============

-   安装主题软件

``` bash
# 主题配置软件 xfce-theme-manager
sudo dnf install xfce-theme-manager.x86_64
```

**安装此软件需要添加 `RPM Fusion` 源**

-   获取安装主题和图标

访问网站[xfce-look](https://www.xfce-look.org/)选择喜欢的主题文件和图标文件，下载解压缩后放在系统相应的目录下

| 项目 | 位置                 | 我的选择 |
|------|----------------------|----------|
| 主题 | `/usr/share/themes/` | axiom    |
| 图标 | `/usr/share/icons/`  | Faenza   |

**如果配置软件无法识别图标，可能是图标目录中有很多套图标，把选中的一套拷贝出来即可。**

打开 `Setting-Xfce Theme Manager` 即可配置。

终端配色和字体
==============

-   终端配色

    我使用的是 `Solarized`配色，本来以为需要到github下载配置，不过不需要了。

    打开终端 `Edit-preferences-colors-presets-Solarized(dark)` 完成

-   终端字体

    下载[powerlinefonts](https://github.com/powerline/fonts)选择自己喜欢的字体，我使用的是`Inconsolata for Powerline` ，如果你需要安装所有的字体，则克隆项目后直接执行安装文件即可。

    我只使用这一个字体就没有安装那么多，单独安装方法：

    -   拷贝字体文件到 `~/.local/share/fonts/` 如果没有自己建一个

    -   执行命令 `fc-cache -f ~/.local/share/fonts` 即可

**参考博客**

[XFCE 桌面环境美化，fedora27系统](http://www.cnblogs.com/pipci/p/8684310.html)
