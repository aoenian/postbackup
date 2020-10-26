---
title: 树莓派最佳伴侣pi-hole安装
date: 2019-02-02
categories: Raspberry Pi
tags:
- pi-hole
- advertisement
---

树莓派放在那里吃灰又快一年了，平时偶尔想起来更新一下系统然后又给关掉了。不过小机器质量还不错，一直很坚挺。这段时间不是太忙了，想着还是让它发挥一下余热。网络搜索以后发现[pi-hole](https://github.com/pi-hole/pi-hole)这个东西不错，下面开工。

<!--more-->

# 需要的装备

1. 硬件：树莓派3b+内存卡+网线（个人感觉无线太慢了）

2. 软件：下载并在内存卡烧录[Raspbian](https://www.raspberrypi.org/downloads/)系统

系统的安装教程在[Installing operating](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)这里可以查看，具体就不再多说了，**要时刻记得，你都是有树莓派的人了**。

# 安装pi-hole

1. 启动树莓派，执行命令`sudo apt update && sudo apt upgrade -y`更新系统。

2. 更新完毕执行如下命令全自动安装pi-hole

    ```bash
    # 安装pi-hole
    sudo curl -sSL https://install.pi-hole.net | bash
    ```

3. 安装过程中会出现选择界面，根据自行需求进行选择设置。下面给出个人建议和选择。

    - 出现 `Pi-hole automated installer` 界面，提示 `This installer will transform....ad blocker`.点击确定

    - `the pi-hole is free, but powered by your donations. http://pi-hole.net/donate` 项目需要大家的捐助（看了一下没有支付宝渠道）。点击确定

    - `the pi-hole is a server so it needs a static ip` pi-hole需要固定ip地址，选个局域网IP地址在路由器上绑定一下。点击确定

    - `select upstream dns provider` 我选择 cloudflare，查了一下这个dns就是那个`1.1.1.1`。

    - 接着选择去广告的list，建议使用默认配置，直接确认即可

    - 然后是IPv4和IPV6选择，先按照默认吧。

    - 因为我的树莓派是dhcp获得的地址，如果你也是一样，这里软件会提示你使用你现在的地址作为静态地址。确认。

    - 确认以后软件会提示你，如果你的局域网依然是使用DHCP自动分配地址可能会发成IP冲突，解决的方法可以在路由器绑定或者把让DHCP地址池不包含树莓派的地址（我用的华为的好像这种方法有问题），这里一般不会出现冲突的情况。

4. 基本的配置完成，后面是安装管理软件。

    - 安装`web admin`，必须安装，图形界面管理

    - 安装`web server`，必须，不安装上面的不管用，自己看着办

    - 是否开启日志，ok开启

    - 隐私模式选择，这个没有看懂，直接默认了

5. 等待软件安装配置完成，然后重启树莓派，浏览器访问 <http://192.168.3.100/admin/> 进行管理。**记得换成你的树莓派IP.**

6. 在管理面板的左侧有`login`按钮，点击登录会提示你输入密码，这个是在安装的时候出现的，不过相信你和我一样没看到，在pi的终端执行如下命令 `sudo pihole -a -p` 重新设置密码。登录后的设置就不再多介绍了。

# 安装过程遇到的问题

这个才是真正需要记录的，估计大部分安装都会遇到。

我在登录管理面板以后，发现pi-hole的状态是没有工作，这时候查看安装记录发现有一个错误，具体错误如下：

```bash
Error: Unable to get latest release location from GitHub
[✗] FTL Engine not installed
```

**解决方法：** 编辑 `/etc/resolv.conf` 文件，把里面的DNS服务器地址由原来的 `127.0.0.1` 更改为 `8.8.8.8` 或者 `1.1.1.1`. 然后执行命令 `pihole -r` 来修复问题，这里建议观察执行后的输出，如果问题依然存在可以再执行一次修复命令。

如果出现 `Enabling pihole-FTL service to start` 则成功！！。

如果去广告列表也出现下载失败，基本解决方法与上边类似，更换DNS服务器地址，然后多执行几次 `pihole -r` 即可。_我这里用的阿里的DNS出现错误，更改为`1.1.1.1`之后解决_。

# 使用pi-hole

把你的盒子的DNS地址更换为树莓派的IP地址即可，享受纯净体验吧！

参考博文：

- [FTL Engine not installed](https://discourse.pi-hole.net/t/ftl-engine-not-installed/9004)

- [Install Pi-hole on Raspbian Lite From Scratch](https://www.myhelpfulguides.com/2018/07/15/install-pi-hole-raspbian-lite/#Install_Pi-hole)