---
title: 手机QQ邮箱日历同步问题
date: 2019-04-30
categories: Android
tags:
- QQ邮箱
- 同步
- 电脑
---

随着临时的工作越来越多，记忆空间的有限，考虑使用日历提醒来记录一些重要的事情。一个是避免忘记，另一个是能够在后面的时候查看日程找到处理事件的时间。

日历的选用经过了这样一个过程，首先是用的手机自带的日历，感觉不方便，同步比较麻烦。然后换用了tim，用了一段时间后发现新建日程的时间只能是将来，不能是过去，有时候把已经干完的事情做一个备忘就很麻烦了。

因为需求较低，只用过三种日历，系统自带、Tim日程、QQ邮箱日历。根据我的使用时的需求对比如下：

<!--more-->

日历 | 主要问题
------- | -------
系统自带 | 更换手机同步不方便
Tim日程 | 无法同步，日程时间只能设定为将来
QQ邮箱日历 | 能够满足基本需求（当时以为能够随帐号同步）

最后呢使用了QQ邮箱客户端的日历，基本符合自己的要求，不过当时天真的以为日历记录会随着帐号同步。可是换手机的时候才发现完全不是那么回事。**手机客户端和网页客户端无法同步。当然客户端之间也无法同步。**

经过一番折腾，连exchange服务都用上了，还给QQ邮箱客户端提了建议，不过回复让我截图之类，就没有再回复，考虑其他的解决方案。**没想到最后解决的方法很简单**，更改一下设置就可以了。

打开安卓QQ邮箱的客户端，右上角的三个点-设置-应用-日历-开启日历同步-默认日历-更改为QQ日历（_默认是选择系统日历，之前不能同步的问题就在这里_）