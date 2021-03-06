---
title: 思科路由器配置1——密码配置
date: 2017-03-12
categories: Net
tags:
- router
- password
---

[配置好的pkt文件](https://github.com/aoenian/cisco-pkt)

密码配置
========

思科的交换机和路由器的密码配置操作是一样的，这里介绍的是交换机中没有涉及到的，但是也可以使用的配置。

<!--more-->

console密码
-----------

``` c
Switch#erase startup-config             // 删除原有配置
Switch#reload                           // 重启交换机
Switch(config)#line console 0
Switch(config-line)#password cisco      // 设置console口登录密码
Switch(config-line)#login               // 开启登录验证
Switch(config-line)#login local  // 开启本地验证，不开启则后面创建的本地用户不能登录

Switch(config-line)#exit
// 创建本地用户以后，从console口登录则需要输入用户名和密码
Switch(config)#username cisco password cisco    // 创建本地用户并设置密码
// 用户权限定义为 0-15 级，最高为15级 任何命令都可操作
Switch(config)#username cisco privilege 15 password cisco  // 该用户登录直接进入特权模式
Switch(config)#service password-encryption  // 把明文密码加密

```

远程ssh登录
-----------

今天看到网络上面介绍 telnet 登录是明文传输，建议采用 `ssh` 方式登录。这里就介绍一下ssh登录的方法

*这里用模拟器测试一下发现，模拟器中支持 domain-name 配置的是三层交换机和路由器，二层交换机无法配置*

``` c
Router(config)#hostname R1841       // 设置hostname
R1841(config)#ip domain-name router.com     // 设置domain-name
R1841(config)#username cisco privilege 15 password cisco  // 创建本地用户和密码
R1841(config)#service password-encryption   // 把密码加密
R1841(config)#enable secret cisco   // 设置特权密码，不然后面远程无法进入特权模式
R1841(config)#crypto key generate rsa   // 生成ssh密钥
The name for the keys will be: R1841.router.com
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024   // 这里需要产生1024位的密钥因为ssh版本2需要位数较多
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

R1841(config)#ip ssh version 2  // 设置使用的ssh版本
*3? 1 0:7:2.224:  %SSH-5-ENABLED: SSH 1.99 has been enabled 
R1841(config)#ip ssh time-out 120   // ssh会话超时时间
R1841(config)#ip ssh authentication-retries 3   // ssh验证的最大次数
R1841(config)#line vty 0 4
R1841(config-line)#transport input all  // 启用所有的认证（telnet和ssh）
R1841(config-line)#login local      // 启用本地验证
R1841(config-line)#exit
R1841(config)#interface f0/0        
R1841(config-if)#no shutdown        // 启动路由器端口
%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up

R1841(config-if)#ip address 192.168.1.1 255.255.255.0   // 设置远程登录IP
R1841(config-if)#exit
R1841(config)#exit
R1841#write     // 写入配置

R1841(config)#crypto key zeroize rsa    // 删除ssh密钥禁止ssh登录
```

测试
----

使用pc连接路由器的f0/0接口，即可在命令行进行登录

```bash
// telnet 登录
PC>telnet 192.168.1.1
Trying 192.168.1.1 ...Open
User Access Verification
Username: cisco
Password: 
// ssh登录
PC>ssh -l cisco 192.168.1.1
Open
Password: 
```

**参考博客**

-   [设置思科设备console密码、enable密码、vty登录密码](http://willy.blog.51cto.com/5093185/1054260)
-   [Cisco路由器SSH登录](http://www.bitscn.com/netpro/router/201403/195093.html)
-   [通过SSH实现Cisco路由器登录](http://www.net130.com/technic/001/20040105003.htm)
