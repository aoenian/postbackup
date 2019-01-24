---
title: Windows7安装VirtualBox失败解决方法
date: 2019-01-05
categories: Windows
tags:
- virtualbox
- error
- 注册表
---

由于需要在windows下面使用linux环境，所以就想着用一下虚拟机做一个完整的系统。_cygwin之类也用过，确实无法驾驭。_ 不如虚拟机来的完整和直接。

不过安装的时候遇到了一些问题，开始是安装不上去，接着是安装以后无法启动，我就郁闷了，在自己的电脑上从没遇到过的情况，一到别人那里就出问题。搜索了一下问题解决了，备忘如下。

<!--more-->

安装的virtualbox版本是`5.2.8-121009`，系统是Win7旗舰版，后来的虚拟机版本好像有时候就没有这种问题。

1. 如果安装不成功，查看电脑中是否有`AutoCAD`，如果确实需要安装虚拟机则需要卸载`AutoCAD`。

    进入 控制面板-卸载程序-卸载 AutoCAD2007

2. 再次安装虚拟机

3. 安装完成后如果启动出问题，建议查看软件出错提示以后再做处理。如果出现获取`VirtualBox COM对象失败`，则可以尝试按照下面的方式修改注册表，再次尝试启动

    - `win+r` 输入 `regedit` 回车

    - 修改注册表

        - HKEY_CLASSES_ROOT\CLSID\{00020420-0000-0000-C000-000000000046} `InprocServer32` 的值修改为  `C:\Windows\system32\oleaut32.dll`

        - HKEY_CLASSES_ROOT\CLSID\{00020424-0000-0000-C000-000000000046} `InprocServer32` 的值修改为 `C:\Windows\system32\oleaut32.dll`

4. 有时候无法启动也可能是安装的虚拟系统与之前的虚拟系统文件重名，具体要注意软件的错误提示
