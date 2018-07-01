---
title: CentOS7安装配置Samba
date: 2017-10-29
categories: Linux
tags:
- samba
- centos
---


前言
====

*性子急的可以跳过，直接看后面的配置*

需求
----

树莓派安装了CentOS7以后感觉还不错，正好大部分学习linux的资料都是CentOS为主，所以就准备长期使用，然后这两天用you-get下载了一些视频，然后想拷贝出来，这时候突然发现CentOS不能写入ntfs格式的U盘，哎，开源和微软之间总有那么一道肉隐肉现的鸿沟。

那么需求来了，让CentOS读取ntfs，搜索一番，发现很简单就是安装一个ntfs-3g的软件的事情，不过就这么个问题废老大劲了。

**2018-06-05更新：32位安装ntfs-3g可[官网](https://www.tuxera.com/community/open-source-ntfs-3g/)下载源码直接编译安装，64位的可以直接安装[EPEL](https://fedoraproject.org/wiki/EPEL)然后再安装相应的软件**

<!--more-->

当时查看系统的位数，找到的一些系统信息查看的方法，总结记录如下。

-   查看CentOS的版本

``` bash
[root@CentOS-rpi3 aoenian]# cat /proc/version
Linux version 4.9.50-v7.1.el7 (mockbuild@armv7-03.dev.CentOS.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC) ) #1 SMP Thu Sep 14 08:17:16 UTC 2017

[root@CentOS-rpi3 aoenian]# uname -a
Linux CentOS-rpi3 4.9.50-v7.1.el7 #1 SMP Thu Sep 14 08:17:16 UTC 2017 armv7l armv7l armv7l GNU/Linux

[root@CentOS-rpi3 aoenian]# uname -r
4.9.50-v7.1.el7

[root@CentOS-rpi3 aoenian]# cat /etc/redhat-release
CentOS Linux release 7.4.1708 (Core)

[root@CentOS-rpi3 aoenian]# getconf LONG_BIT
32

[root@CentOS-rpi3 aoenian]# file /bin/ls
/bin/ls: ELF 32-bit LSB executable, ARM, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=c591c4039d5eb3301eb8be1fa4ebaed8cd832240, stripped

```

安装配置Samba
=============

CentOS的共享比较麻烦具体下面介绍

安装Samba
---------

先查看是否安装Samba

``` bash
rpm -qa | grep samba
yum list installed | grep samba
```

如果已经安装那么可以跳过，如果没有正确安装可以先删除再重新安装

``` bash
# 删除samba
yum remove samba*
```

接着，连上网络，安装samba

``` bash
yum install samba* -y
```

配置一个完全权限的共享目录
--------------------------

### 查看windows的工作组

打开win的cmd窗口，快捷键 win+r 然后输入 cmd
回车即可，并在窗口输入以下命令

``` bash
net config workstation
```

找到 `Workstation domain` 一行，值应该是 `WORKGROUP` 下面配置Samba要用到

### 配置Samba

-   创建共享目录

``` bash
mkdir -p /samba/anonymous
chmod -R 0777 /samba/anonymous
chown -R nobody:nobody /samba/anonymous
```

-   关掉防火墙和selinux

``` bash
systemctl stop firewalld.service #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动

setenforce 0    # 关闭selinux
getenforce      # 查看selinux的状态，应该是 Permissive

# 彻底禁用selinux 编辑配置文件
vim /etc/sysconfig/selinux    
# 修改SELINUX
SELINUX=disabled
```

*如果防火墙不关闭，查看共享会出现连接失败；如果selinux不关闭，则无法看到共享目录的内容*

*当然为了安全期间，可以对Samba的端口放行，设置selinux分享Samba的目录，大家可以参考其他博文*

-   备份配置文件

``` bash
cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

-   修改配置文件

``` bash
vi /etc/samba/smb.conf

# 可以把之前的内容注释掉，加入如下内容
[global]
        workgroup = WORKGROUP    # 跟win的工作组同名
        server string = Samba Server Version %v    # 服务器描述
        netbios name = CentOS    # 共享服务器在网络上显示的名字
        security = user    # 使用 SAMBA 服务器本身的密码数据库
    # 将所有Samba系统主机所不能正确识别的用户都映射成guest用户
        map to guest = bad user    
        dns proxy = no
        log file = /var/log/samba/log.%m    # 日志文件位置
        max log size = 50    # 日志大小

[Anonymous]
        comment = Anonymous File Server Share
        path = /samba/anonymous
        browsable = yes    # 让所有的用户看到这个项目
        writable = yes    # 是否可以写入
        guest ok = yes    # 是否允许guest账户访问
        read only = no    # 是否只读，与wirtable类似，谁在后面听谁的
```

*保存配置文件，并退出，到此一个完全共享的目录基本配置完毕*

### 重启smaba

``` bash
systemctl enable smb.service    # 加入开机启动
systemctl enable nmb.service
systemctl restart smb.service
systemctl restart nmb.service
```

*配置完成，可在其他系统查看共享目录和文件*

参考
====

-   [Easy Samba installation on RHEL/CentOS7](https://lintut.com/easy-samba-installation-on-rhel-centos-7/)
-   [Install And Configure Samba Server In CentOS7](https://www.unixmen.com/install-configure-samba-server-centos-7/)
-   [Samba常用配置及GUEST访问](http://billtym.blog.51cto.com/1745172/569551)
-   [设置 SAMBA](https://wiki.centos.org/zh/HowTos/SetUpSamba)
-   [鸟哥文件服务器](http://cn.linux.vbird.org/linux_server/0370samba_2.php)
