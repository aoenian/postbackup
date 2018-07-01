---
title: 思科交换机配置3——多交换机VLAN划分
date: 2017-03-05
categories: Net
tags:
- switchs
- VLAN
---

配置好的思科模拟器文件地址：[cisco-pkt](https://github.com/aoenian/cisco-pkt/)

拓扑图:
![](https://raw.githubusercontent.com/aoenian/postbackup/master/topupic/switch3.PNG)

<!--more-->

s1配置
======

``` c
Switch(config)#vlan 10
Switch(config-vlan)#name v10
Switch(config-vlan)#exit
Switch(config)#vlan 20
Switch(config-vlan)#name v20
Switch(config-vlan)#exit
Switch(config)#interface f0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit
Switch(config)#interface f0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#exit
Switch(config)#interface f0/24
Switch(config-if)#switchport mode trunk     // 把24端口模式设置为trunk来进行交换机之间的通信

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/24, changed state to up

Switch(config-if)#exit
Switch(config)#exit
Switch#write
Building configuration...
[OK]

```

s2的配置
========

**s2的配置和s1的配置几乎完全一样，这里只列出配置过程，不再注释**

``` c
Switch(config)#hostname s2  // 把交换机的名称改为s2
s2(config)#interface f0/1
s2(config-if)#exit
s2(config)#vlan 10
s2(config-vlan)#name v10
s2(config-vlan)#exit
s2(config)#vlan 20
s2(config-vlan)#name v20
s2(config-vlan)#exit
s2(config)#interface f0/1
s2(config-if)#switchport mode access 
s2(config-if)#switchport access vlan 10
s2(config-if)#exit
s2(config)#interface f0/2
s2(config-if)#switchport mode access 
s2(config-if)#switchport access vlan 20
s2(config-if)#exit
s2(config)#interface f0/24
s2(config-if)#switchport mode trunk 

```

测试
====

pc1与pc3可以通信但与pc2无法通信

pc2与pc4可以通信但与pc3无法通信

**参考链接以及拓展阅读**

-   [Cisco基础【交换机】两个实例配置让你掌握交换机VLAN的划分](http://zhaoyuqiang.blog.51cto.com/6328846/1576016)

-   [跨交换机的Vlan配置与管理](https://learningnetwork.cisco.com/docs/DOC-9289)
