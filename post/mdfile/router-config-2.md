---
title: 思科路由器配置2——单臂路由
date: 2017-04-17
categories: Net
tags:
- router
- one-armed
---


[单臂路由](http://baike.baidu.com/item/%E5%8D%95%E8%87%82%E8%B7%AF%E7%94%B1):是指在路由器的一个接口上通过配置子接口（或“逻辑接口”，并不存在真正物理接口）的方式，实现原来相互隔离的不同VLAN（虚拟局域网）之间的互联互通。

[配置好的pkt文件](https://github.com/aoenian/cisco-pkt)

<!--more-->

拓扑图：
![](https://raw.githubusercontent.com/aoenian/postbackup/master/topupic/router2.png)

交换机S0配置
============

``` c
Switch(config)#hostname S0
S0(config)#exit
S0#vlan database        // 进入vlan数据库模式
S0(vlan)#vlan 10 name v10   // 创建vlan10并命名为v10
VLAN 10 modified:
    Name: v10
S0(vlan)#vlan 20 n v20      // 创建vlan20
VLAN 20 added:
    Name: v20
S0(config)#interface r f0/1-10  // 把端口1-10划分给vlan10
S0(config-if-range)#swi mode acc    // 更改端口模式为 access
S0(config-if-range)#swi access vlan 10  // 端口加入vlan10
S0(config-if-range)#exit
S0(config)#interface r f0/11-20
S0(config-if-range)#swi mode acc
S0(config-if-range)#swi acc vlan 20

S0(config)#inter f0/24      // 设置与路由器R0连接的端口24
// 这里由于交换机只支持802.1q协议，所以没有设置协议，只设置模式
S0(config-if)#swi mode trunk 
S0(config-if)#swi trunk allowed vlan all    // 允许所有vlan通过
```

PC的设置
========

| PC  | vlan | IP              | gateway          |
|-----|------|-----------------|------------------|
| PC0 | 10   | 192.168.1.11/24 | 192.168.1.254/24 |
| PC1 | 20   | 192.168.2.11/24 | 192.168.2.254/24 |

路由器R0的配置
==============

``` c
Router>enable
Router#conf
Configuring from terminal, memory, or network [terminal]? 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R0
R0(config)#inter f0/0   // 进入路由器和交换机相连的端口
R0(config-if)#no shutdown   // 启用端口
R0(config-if)#exit
R0(config)#inter f0/0.1     // 进入逻辑子接口0.1
R0(config-subif)#encapsulation dot1Q 10     // 设置协议封装并建立与vlan10的关联
R0(config-subif)#ip add 192.168.1.254 255.255.255.0 // 设置ip地址（也就是网关地址）
R0(config-subif)#exit
R0(config)#inter f0/0.2
R0(config-subif)#encapsulation dot1Q 20
R0(config-subif)#ip add 192.168.2.254 255.255.255.0
R0(config-subif)#exit
R0(config)#exit
R0#write memory     // 写入配置
Building configuration...
[OK]

```

测试
====

PC0与PC1之间能够互相ping通。

参考博客
========

-   [Cisco单臂路由配置，图文实例详解](http://blog.csdn.net/junmuzi/article/details/49912217)
-   [单臂路由](http://baike.baidu.com/item/%E5%8D%95%E8%87%82%E8%B7%AF%E7%94%B1)
