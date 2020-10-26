---
title: 树莓派安装UbuntuServer并设置Samba共享
date: 2020-05-08 21:53:40
categories: Raspberry Pi
tags:
- ubuntu
- 密码共享
---

用来用去还是感觉Ubuntu省心，很多配置没那么复杂，有时候也觉得自己挺可笑的，有时候连基本的应用都还没熟练，还去考虑哪个系统稳定，简直操心太多。

这篇文章记录树莓派安装Ubuntu Server并配置文件共享的过程。

## ubuntu Server安装

### 软硬件准备

- ubuntuserver镜像（我没安装图形界面的，个人觉得不需要）网址：<https://ubuntu.com/download/raspberry-pi> 这里我用的是20.04 64位
- Raspberry Pi 3B + Micro SD卡
- 烧录软件：balenaEtcher 这个挺好用网址 <https://www.balena.io/etcher/>

<!--more-->

烧录可以参考官方教程：<https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview>  

### 启用ssh

在烧录以后把卡插入树莓派中即可启动，因为不想再插入键盘和鼠标进行设置，所以需要在系统中启用远程ssh。启动方法：**在根目录下新建一个空白文件名字为ssh即可，无需后缀**。

### ssh连接树莓派

通过路由器知道你的树莓派的IP地址（最好用路由给PI分配一个固定的IP），然后直接输入以下命令进行连接：

```bash
# ubuntuserver的默认用户名和密码都是 ubuntu
# 第一次登录会让你修改
# mac和windows10的powershell
ssh ubuntu@<raspberrypi IP address>
```

注意：如果你的电脑之前连接过树莓派可能需要删除之前的密钥重新连接才能继续。

## samba软件安装设置

### 安装samba

建议安装前更换官方的软件源，因为真的太慢了。详细的可以参考中科大的帮助文件，地址<http://mirrors.ustc.edu.cn/help/ubuntu.html>

```bash
# 更换源 记得备份原来的
sudo sed -i 's/ports.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
# 更新软件源和软件
sudo apt update && sudo apt upgrade -y
# 安装samba
sudo apt install samba smbclient samba-common
```

### 修改smb.conf

依然记得首先要备份，然后再修改，这里设置两种共享，一种加密，另一种不加密。
配置文件的位置 `/etc/samba/smb.conf`。

注：**不建议在/tmp目录下使用，因为重启后会自动清空。**如需更改请查看文末的链接资料

```bash
# 不加密的共享目录 仅做测试用，建议更换目录
[temp]  # 显示的名称
    comment = Temporary file space  # 说明
    path = /tmp/share   # 共享目录的位置，记得权限全部开放才能读写
    read only = no      # 是否只读
    browseable = yes    # 可以浏览
    guest ok = yes      # 允许访客访问

# 保存退出后使用下面命令检测配置
testparm    # 检测配置文件

sudo service smbd restart   # 重启服务

# 加密目录设置 这个也是在 smb.conf 中配置
[share]
    comment = home files share
    path = /home/share
    read only = no
    browsable = yes

# 配置完成后保存退出，命令检测如上所示
# 增加能够访问共享的用户，需要添加系统已存在的用户
# username可以用ubuntu替换，会提示设置密码，这个是访问共享的密码
sudo smbpasswd -a username
# 完事收工
```

### 遇到的问题

主要是mac系统访问出现问题，访问加密目录的时候出现如下提示`The operation can't be completed because the original item for "share" can't be found.`

解决方法是打开finder，快捷键`command+k`，也就是连接服务器，然后输入 `smb://PI'sIP`即可。因为mac默认使用的是AFP协议。

参考资料：

- <http://linux.vbird.org/linux_server/0370samba.php#theory_daemons>

- [官方的指南](https://ubuntu.com/tutorials/install-and-configure-samba#3-setting-up-samba)

- <https://raspberrypi.stackexchange.com/questions/58185/how-do-i-share-files-with-a-mac>

- <https://stackoverflow.com/questions/13669585/how-to-get-a-list-of-all-valid-ip-addresses-in-a-local-network>

- <https://askubuntu.com/questions/625455/why-is-tmp-getting-cleaned-out-in-ubuntu-15-04>
