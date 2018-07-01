---
title: 思科交换机配置1——密码配置
date: 2017-03-04
categories: Net
tags:
- switch
- password
---

**交换机的配置将会是一个系列文章，我会把自己配置过的例子都写出来做一个系列，也避免之后每次遇到都要去网络搜索。使用 packet tracer 的版本号为6.2(5.3版本会出现问题)，查看此文需要对思科模拟器有一定的了解。**

<!--more-->

仿真终端配置
------------

| bps  | Data Bits | parity | stop bits | flow control |
|------|-----------|--------|-----------|--------------|
| 9600 | 8         | None   | 1         | None         |

交换机模式介绍
--------------

### 用户模式

刚进入交换机时命令行状态为 `Switch>`，此时所处为用户模式，这个模式一般用于查看显示系统信息、改变终端设置和执行一些最基本的测试命令，如ping、traceroute等

**注意：直接输入 `?`并回车，可获得该模式下允许执行的命令。其他模式同样适用**

### 特权模式

用户模式下，执行 `enable` 命令，可能需要输入密码，密码输入时不回显，完成后直接回车即可。

特权模式的命令行状态为： `Switch#`

离开特权模式的命令为： `exit` 或 `disable`

### 全局配置模式

在特权模式下，执行 `configure  terminal` 命令，即可进入全局配置模式。在该模式下，只要输入一条有效的配置命令并回车，内存中正在运行的配置就会立即改变生效。该模式下的配置命令的作用域是全局性的，是对整个交换机起作用。

全局配置模式的命令行状态为： `Switch(config)#`

在全局配置模式，还可进入接口配置、line配置等子模式。从子模式返回全局配置模式，执行 exit 命令；从全局配置模式返回特权模式，执行exit命令；若要退出任何配置模式，直接返回特权模式，则要直接end命令或按Ctrl+Z组合键。

### 接口配置模式

在全局配置模式下，执行 `interface` 命令，即进入接口配置模式。在该模式下，可对选定的接口（端口）进行配置，并且只能执行配置交换机端口的命令。

接口配置模式的命令行提示符为： `Switch(config-if)#`

### line配置模式

在全局配置模式下，执行 `line vty` 或 `line console` 命令，将进入Line配置模式。该模式主要用于对虚拟终端（vty）和控制台端口进行配置，其配置主要是设置虚拟终端和控制台的用户级登录密码。

Line配置模式的命令行提示符为： `Switch(config-line)#`

**其他的模式在后面使用的时候再进行介绍，参考文档[思科交换机的配置模式和命令](https://wenku.baidu.com/view/4867dc4d767f5acfa1c7cd70.html)**

交换机初始配置
--------------

*初始的交换机是没有任何密码，也不能远程登录，所以首次配置需要使用 console 控制线进行配置*

注意：下面所有的密码配置都是 `cisco`

### 配置console口密码

``` c
Switch>enable       // 进入特权模式
Switch#config t     // 进入全局配置模式
Enter configuration commands, one per line.  End with CNTL/Z.   
Switch(config)#line console 0       // 进入console口配置
Switch(config-line)#password cisco  // 设置密码
Switch(config-line)#login       // 启用密码 很重要
Switch(config-line)#end     // 退出到特权模式，也可以使用 exit
Switch#copy running-config startup-config   // 把刚才的配置保存，避免重启后丢失
Switch#reload       // 重启 验证
```

### 配置特权密码

``` c
// 特权密码有两种设置方法，password 和 secret ，建议使用 secret 设置
// secret 设置优先级高，而且密码会使用密文显示
Switch(config)#enable ?     // 两种密码设置方式
  password  Assign the privileged level password
  secret    Assign the privileged level secret

Switch(config)#enable secret cisco  // 在全局模式下设置

// 记得验证然后写入配置
```

### 配置远程登录密码（telenet）

如果每次配置交换机都需要用配置线在交换机的配置口配置，那估计要疯掉了，所以我们需要配置远程登录。

``` c
// 因为远程配置需要ip地址，所以需要给交换机配置管理ip
Switch(config)#interface vlan 1     // 进入交换机的默认 1 号vlan
Switch(config-if)#ip address 192.168.1.1 255.255.255.0  // 设置管理IP
Switch(config-if)#no shutdown       // 启用vlan

%LINK-5-CHANGED: Interface Vlan1, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

Switch(config-if)#exit      // 退出到全局模式
Switch(config)#line vty 0 4 // 配置远程虚拟接口 0 4 指5个接口 0到4
Switch(config-line)#password cisco  // 设置密码
Switch(config-line)#login       // 启用密码
Switch(config-line)#end     // 退出
```

然后可以通过在一个局域网的电脑进行网络链接，在电脑的 cmd 命令行中执行如下命令：

`telnet 192.168.1.1` 回车输入密码即可。这里需要注意的是，如果通过telnet登录的话，不仅需要远程telnet登录密码，还需要设置特权密码，不然的话是不能进入特权模式的。
