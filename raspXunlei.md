---
title: 树莓派下载系列（一）————迅雷远程
date: 2016-10-01 11:24:04
categories: Raspberry Pi
tags: download
---

### 准备工作

树莓派一个（已经安装好raspberrypi系统）

迅雷帐号一个（需要用来远程管理）

*经过使用，我的是电信宽带10M，树莓派3，迅雷普通会员，但是没什么卵用，下载依然具慢，而且十分容易死机。所以大家不要报太大的希望，和电脑绝对是两码事。*

<!-- more -->

### 开工

参考教程：[树莓派安装迅雷远程下载][1] 里面有些内容不适合新版的系统具体可参考下面的介绍。
[1]: http://luyou.xunlei.com/thread-430-1-1.html?_t=1466157534

+ 设定root密码并打开root用户登录限制

教程请查看[这里](http://www.shumeipaiba.com/wanpai/jiaocheng/2.html)

默认登录名为： pi 

默认登录密码为：raspberry （建议等会儿改成简单的，太麻烦）

登入系统以后打开终端，如果ssh的话就只有终端界面，执行命令

    # 设定root用户密码
    sudo passwd   # 回车以后输入两次root密码（密码不会显示）

    # 打开root用户登录限制,因为默认是不允许root用户直接登录
    sudo passwd --unlock root  # 但是你会发现新版的树莓派系统这个命令不行了接着看下面

    # 需要修改ssh的配置文件 这里我用vi 你也可以用 nano
    sudo vi /etc/ssh/sshd_config

    # 把PermitRootLogin 的参数更改为 yes
    PermitRootLogin yes

    # 重启即可用root用户登录
    sudo reboot

    # 修该pi用户密码 根据自己需要
    sudo passwd pi

+ 挂载u盘

这里需要注意，新版的系统默认已经安装ntfs-3g模块，如果没有请执行

`sudo apt-get install ntfs-3g   # 新版系统好像已经安装`

这里我使用的是手动挂载u盘，一个原因是上面教程中的自动挂载没有用，另一个我的树莓派并不是下载为主，只是偶尔使用。

需要自动挂载可以查看[这里](https://blog.gtwang.org/iot/raspberry-pi-auto-mount-usb-disk/)
不过我并没有验证是否有效，后面验证完毕后会更新这里的内容。

下面介绍手动挂载

    # 插入u盘后，查看dev 如果出现sda1 则说明u盘已经识别
    ls /dev/sd*
    # 在/mnt 目录下建立挂载目录
    sudo mkdir /mnt/usb
    # 挂载
    sudo mount -t ntfs /dev/sda1 /mnt/usb
    # 卸载
    sudo umount /mnt/usb


然后就可在/media/usb 目录下看到u盘的文件。具体的mount命令使用可查看[mount][m]
[m]: http://www.runoob.com/linux/linux-comm-mount.html

+ 安装迅雷程序

先把下载的迅雷文件的压缩包解压出来，然后放在u盘里面，文件下载地址：[点我][2]
[2]: http://luyou.xunlei.com/thread-12545-1-1.html
树莓派请下载： Xware1.0.31_armel_v5te_glibc.zip 然后把u盘插入树莓派 

    # 当然先挂载，然后进入u盘目录，我把迅雷的名字更改,纯粹为了方便
    mv Xware1.0.31_armel_v5te_glibc xunlei
    # 启动远程下载
    ./portal

接着会出现` THE ACTIVE CODE IS: ****** ` 星号代表你的设备码。然后登录远程迅雷在面板左侧添加设备，把设备码输入即可。

[远程迅雷地址](http://yuancheng.xunlei.com/login.html#)

### 到这里基本就完成了，至于速度就是你的资源热度和网速了。

**注：由于迅雷远程会查看文件的版权以及是否非法，所以如果有些无法下载的种子或磁力，建议使用 transmission-cli 下载。**
