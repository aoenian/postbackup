---
title: 网件R6400V2固件升级导致小米盒子无法联网
date: 2019-07-17
categories: Net
tags:
- netgear
- 小米
- mibox
---

今天进入路由器更改一些参数，发现路由器提醒有新的固件可以升级，固件版本是`1.0.4.78_10.0.55`，想着新的固件应该能够让路由器更稳定运行就随手点击更新。然后悲剧就来了。

<!--more-->

中午更新的系统，然后就休息了，下午起来后打开小米盒子看电视的时候发现不对了，网络连接都正常，就是看不到视频，提示**网络未连接**，我就郁闷了。

## 检查过程

- 检测家庭其他设备，联网正常
- 重启小米盒子
- 重启路由器
- 重置小米盒子
- 重置路由
- 用手机开热点，连接后没有问题，看来确实是路由器的问题

其实在重置小米盒子后就感觉应该是路由器升级固件导致的，不过嫌麻烦想着如果重置能够搞定就不折腾了。但是经过上面的处理后盒子依然无法观看，进行网络诊断提示**画报CDN资源访问出现问题**，盒子给出的建议是换个路由器，我已经无法表达小米盒子的智能了。

## 搜索原因

在网络查看后发现很多都反应网件路由和小米盒子连接出问题。既然升级固件后出现的，决定重刷固件降级回之前的版本。

## 降级

在降级之前，也想过刷梅林固件，方法比较复杂，而且想到去广告我有树莓派，基本用不到路由器，路由器最好就像不存在一样，稳定最好。最后决定还是降级为官方固件。

1. 在[R6400v2](http://support.netgear.cn/doucument/More.asp?id=2365)这里下载旧的固件版本，我用的`1.0.2.62_10.0.46`。

2. 按照[升级指南](http://support.netgear.cn/kb_web_files/router10120.htm)，进行操作即可。**这里指南中要求是升级后复位，不过我这边升级后即可使用，并不需要复位。**

降级完毕后，小米盒子马上恢复，这次经验就是：_如果路由器很正常，就别折腾人家了_。