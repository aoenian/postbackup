---
title: 思科交换机配置2——单交换机VLAN划分
date: 2017-03-05
categories: Net
tags:
- VLAN
- switch
---


单交换机VLAN配置
================

配置好的思科模拟器文件地址:[cisco-pkt](https://github.com/aoenian/cisco-pkt/)

配置参数

| 名称 | IP地址          | 所属VLAN | 连接端口 | 网关              |
|------|-----------------|----------|----------|-------------------|
| PC1  | 192.168.1.11/24 | VLAN10   | f0/1     | 192.168.1.254/24  |
| PC2  | 192.168.1.12/24 | VLAN10   | f0/2     | 192.168.1.254/24  |
| PC3  | 192.168.2.11/24 | VLAN20   | f0/11    | 192.168.2.254/24 |
| PC4  | 192.168.2.12/24 | VLAN20   | f0/12    | 192.168.2.254/24 |

<!--more-->

创建VLAN
--------

*为了不占用太大的篇幅，一些命令的常规提示将不会写出来*

### 全局模式下配置

``` c
Switch#configure terminal   
Switch(config)#vlan 10      // 创建vlan 10
Switch(config-vlan)#name v10    // 为 vlan10 命名
Switch(config-vlan)#exit
Switch(config)#exit
```

### VLAN database 模式

``` c
Switch#vlan database 
Switch(vlan)#vlan 20 name v20
VLAN 20 added:
    Name: v20
```

把端口加入相应的VLAN
--------------------

端口的配置可以配置单个端口，也可以多个端口一块配置

### 把1\~10号端口加入vlan10

``` c
Switch(config)#interface f0/1       // 设置端口1
Switch(config-if)#switchport mode access    // 设置端口模式
Switch(config-if)#switchport access vlan 10 // 把此端口加入vlan10中
Switch(config-if)#exit          // 退回全局配置
Switch(config)#interface range fastEthernet ?   // 查看帮助
  <0-9>  FastEthernet interface number
Switch(config)#interface range fastEthernet 0/2-10  // 设置2~10端口
Switch(config-if-range)#switchport mode access      // 设置模式
Switch(config-if-range)#switchport access vlan 10   
Switch(config-if-range)#exit

```

### 把11-20号端口加入vlan20

``` c
Switch(config)#interface range f0/11-20
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 20
```

把配置写入
----------

``` c
Switch#write        // 一种方式
Switch#copy running-config startup-config   // 另一种方法
Destination filename [startup-config]? 
Building configuration...
[OK]
Switch#
```

查看vlan的配置
--------------

``` c
Switch#show vlan    // 查看vlan配置

Switch#show running-config  // 查看交换机配置
```

测试
----

pc1与pc2可以互相ping通，与pc3和pc4无法ping通

更改pc3或pc4与pc1为一个网段的IP，依然无法与pc1通信。
