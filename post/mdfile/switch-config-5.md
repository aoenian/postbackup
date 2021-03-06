---
title: 思科交换机配置5——链路聚合 
date: 2017-03-18
categories: Net
tags:
- 链路聚合
- 三层
---

**这个配置一定使用6.0以上的版本，我使用5.3的版本可以聚合成功，但是当链路只剩一条时显示绿色，却无法通信，浪费了很多时间。**

[PT6.2下载](http://www.xiazaiba.com/html/28845.html)

[配置好的pkt文件](https://github.com/aoenian/cisco-pkt)

<!--more-->

二层交换机链路聚合配置
======================

``` c
// 用两台2950，需要聚合的端口为f0/1-3
S1(config)#interface range f0/1-3
S1(config-if-range)#channel-group 1 mode desirable  // 链路聚合加入通道组1并设置相应模式
S1(config-if-range)#switchport mode trunk   // 端口模式设置

// S2设置与S1类似
Switch(config)#hostname S2
S2(config)#interface range f0/1-3
S2(config-if-range)#channel-group 1 mode desirable 
S2(config-if-range)#switchport mode trunk 

```

三层交换机链路聚合配置
======================

``` c
Switch(config)#hostname S3
S3(config)#interface range g0/1-2
S3(config-if-range)#channel-group 1 mode active     // 使用LACP协议
S3(config-if-range)#switchport trunk encapsulation dot1q // 802.1q协议封装
S3(config-if-range)#switchport trunk allowed vlan all   // 允许vlan通过

// S4的配置
Switch(config)#hostname S4
S4(config)#interface r g0/1-2   // 与上面的配置相同，不过这里简写了
S4(config-if-range)#channel-g 1 mode active 
S4(config-if-range)#swit trunk e dot1q 
S4(config-if-range)#swit t a vlan all

```

三层以太通道的配置
==================

``` c
// 主要是为 port-channel 配置IP地址
// 用两台3560 聚合端口为 g0/1 和 g0/2
Switch>enable
Switch#configure 
Configuring from terminal, memory, or network [terminal]? 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#ip routing
Switch(config)#int range g0/1-2
Switch(config-if-range)#no switchport 
Switch(config-if-range)#channel-group 1 mode active 
Switch(config-if-range)#exit
Switch(config)#int port-channel 1
Switch(config-if)#no switchport 
Switch(config-if)#ip add 192.168.1.1 255.255.255.0
```

参考博客
========

-   [cisco链路聚合配置](http://wugang2126.blog.51cto.com/329386/1161801)
