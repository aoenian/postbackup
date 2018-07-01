---
title: 解决Win10安装Office 2013出现Error 2203错误
date: 2016-08-19 17:08:26
categories: Windows
tags:
- office2013
- error 2203
---

由于不是办公电脑，所以之前一直用的是LibreOffice，金山的WPS也用过，因为广告比较多家人容易误点所以卸载了。Libreoffice的界面虽然没有微软的好看，不过功能一点都不少，而且兼容性作为普通的浏览是没有问题的，促使我更换的原因是启动太慢的问题。

<!-- more -->

LibreOffice打开文档的速度太慢了，这个原因我估计也不在软件，可能系统的问题比较大，我在Linux里面也用过速度是很快的。当然电脑的配置也是一个重要的原因。不过每次打开的时间确实太长了，而且第一次打开后，第二次接着打开需要的时间几乎一样。后来还是妥协了，换回了微软自己的产品，这里没有再尝试OpenOffice.感兴趣的可以测试一下OpenOffice在Win下面表现如何。

罗嗦的太多了，进入正题，在安装Office2013的过程中出现了`Error 2203`错误，之前从没遇到过，网络搜索以后发现出现这个问题的还不少。

解决的方法参考下面的博客：[博客链接](http://www.cnblogs.com/navybcloud/articles/1915236.html)

**处理方法：在当前的用户变量中编辑TEMP变量，在最后增加变量值 `%USERPROFILE%\Local Settings\Temp` 即可解决。记得变量之间要用`;`隔开。**


