---
title: 思科交换机配置4——三层交换机VLAN联通
date: 2017-03-08
categories: Net
tags:
- 三层交换机
- 链路聚合
---

**三层和二层交换机的链路聚合没有成功。可以通信，但聚合链路中拿走一条链路后则通信中断，这个是PT5.3版本的坑！！**

**PT5.3这个大坑我上来就踩上了，这个版本的链路聚合有问题，当只有一条链路的时候，无法ping通。所以直觉就是链路聚合没有配置成功。请用6.2版本。**

拓扑图如下：
![](https://raw.githubusercontent.com/aoenian/postbackup/master/topupic/switch4.png)

<!--more-->

配置好的思科模拟器文件地址：[cisco-pkt](https://github.com/aoenian/cisco-pkt/)

二层交换机配置
==============

S1和S2的配置类似与之前的[跨交换机vlan划分](https://aoenian.github.io/2017/03/05/switch-config-3/)这里不再重复，需要的配置如下：

-   在S1和S2上划分vlan10和vlan20
-   把相应的端口加入vlan
    -   PC2-S1-f0/2 PC4-S2-f0/2 vlan20
    -   PC1-S1-f0/1 PC3-S2-f0/1 vlan10
-   把PC的IP地址和网关地址配置完成

三层交换机S0的配置
==================

三层交换机和二层交换机的接口
----------------------------

|     | S1          | S2    |
|-----|-------------|-------|
| S0  | f0/22 f0/23 | f0/24 |

注：这里我把S1与S0之间用两条链路主要为了配置链路聚合。

S0的具体配置
------------

``` c
Switch>enable
Switch#configure terminal 
Switch(config)#hostname S0
S0(config)#vlan 10
S0(config-vlan)#name v10
S0(config-vlan)#exit
S0(config)#vlan 20
S0(config-vlan)#name v20
S0(config-vlan)#exit
S0(config)#interface vlan 10
S0(config-if)#ip address 192.168.1.254 255.255.255.0    // 设置vlan的网关地址
S0(config-if)#no shutdown       // 启用vlan
S0(config-if)#exit
S0(config)#interface vlan 20
S0(config-if)#ip address 192.168.2.254 255.255.255.0
S0(config-if)#no shutdown 
S0(config-if)#exit
S0(config)#ip routing       // 启用ip路由
S0#show ip route        // 查看三层交换机路由表

```

注：这个时候如果二层交换机和三层交换机连接的接口模式都已经配置为 `trunk`，vlan间即可通信。

S1和S0之间聚合链路的配置
------------------------

首先配置S0路由器的 22 和 23号端口，这里在网络上搜索的配置不尽相同，有多有少。所以这里根据我自己的理解进行配置，如果不正确或者需要补充，请大家留言或通过邮件联系我，谢谢！

### S0三层交换机的配置

``` c
S0(config)#interface range f0/22-23
S0(config-if-range)#channel-protocol pagp   // 设置聚合协议为pagp
S0(config-if-range)#channel-group 1 mode auto   // 设置工作模式
S0(config-if-range)#exit
S0(config)#interface port-channel 1
S0(config-if)#switchport trunk encapsulation dot1q // 802.1Q封装
S0(config-if)#switchport mode trunk     // 更改端口模式
S0(config-if)#switchport trunk allowed vlan all     // 允许vlan通过

S0(config)#interface f0/24      // 与S2的相连端口设置
S0(config-if)#switchport trunk allowed vlan all
S0(config-if)#switchport trunk encapsulation dot1q 

```

### S1的配置

``` c
S1(config)#interface range f0/22-23
S1(config-if-range)#channel-protocol pagp 
S1(config-if-range)#channel-group 1 mode auto
S1(config-if-range)#exit
S1(config)#interface port-channel 1
S1(config-if)#switchport trunk allowed vlan all
S1(config-if)#switchport mode trunk 

S1#show etherchannel summary 

```

### S2配置

``` c
S2#configure 
S2(config)#interface f0/24
S2(config-if)#switchport mode trunk
S2(config-if)#switchport trunk allowed vlan all


```

测试
----

-   不同vlan之间的pc可以互相通信
-   S1和S0之间的两条链路都可传输信息，
    **但是如果去除一条链路和无法通信**

参考链接
--------

-   [通过三层交换实现VLAN间互通的方法(图文教程)](http://www.jb51.net/softjc/56598.html)
-   [Packet Tracer 5.0实验(四)
    利用三层交换机实现VLAN间路由](http://www.cnblogs.com/mchina/archive/2012/07/14/2591598.html)
-   [在第三层交换机的配置VLAN间路由](http://www.cisco.com/c/zh_cn/support/docs/lan-switching/inter-vlan-routing/41860-howto-L3-intervlanrouting.html)

拓展阅读
--------

-   [VLAN/Trunk以及三层交换](http://blog.csdn.net/dog250/article/details/8219141)
-   [链路聚合、Trunk、端口绑定和捆绑简析](http://www.maiziedu.com/article/11162/)
-   [以太网通道及链路聚合协议](https://gnu4cn.gitbooks.io/ccna-60d/content/d33-EtherChannels-and-Link-Aggregation-Protocols.html)
