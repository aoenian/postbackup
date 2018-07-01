---
title: 查看树莓派版本
date: 2018-05-20
categories: Raspberry Pi
tags:
- versions
---

安装软件的时候想查看树莓派系统是32位还是64位就出现了以下的操作

具体命令及作用如下：

``` bash
getconf LONG_BIT        # 查看系统位数
uname -a            # kernel 版本
/opt/vc/bin/vcgencmd  version   # firmware版本
strings /boot/start.elf  |  grep VC_BUILD_ID    # firmware版本
cat /proc/version       # kernel
cat /etc/os-release     # OS版本资讯
cat /etc/issue          # Linux distro 版本
cat /etc/debian_version     # Debian版本编号
```

*虽然树莓派3b的硬件支持64位的系统，但是官方的系统还是32位的，主要应该是为了兼容之前的硬件*

**参考**

-   [如何看 Raspbian的版本資訊？](https://www.raspberrypi.com.tw/10400/check-what-raspbian-version-you-are-running-on-the-raspberry-pi/)
