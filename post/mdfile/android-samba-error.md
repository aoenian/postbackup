---
title: 安卓手机访问树莓派samba文件共享出错解决
date: 2020-09-12 16:52:16
categories: Android
tags:
- samba
- error
---

这段时间树莓派一直没有用，不过后来想想还是用起来，不然就浪费了，当然还是用做家庭的文件共享最好。

## samba配置

这个网上的配置很多，我在之前的博客中也记录过，这里我用的是[ubuntu server](https://www.raspberrypi.org/downloads/)。因为用习惯了，方便。

配置过程就不多说了。看以看看这个 [samba文件共享](https://aoenian.github.io/2020/05/08/rasp-ubuntuserver-samba/)

<!--more-->

## 局域网访问

### pc平台

这个没啥问题，Windows直接使用 `\\树莓派ip地址` 就可以看到共享的文件夹。

mac稍微麻烦一点，打开finder然后点击前往-连接服务器-在里面填写 smb://192.168.3.50 -点击连接-如果你设置的是匿名登录的话。连接身份选择客人，然后连接即可看到共享。

### Android平台

这个就麻烦了，之前用的是es文件浏览器，但是现在不行了，局域网能够扫描出来，但访问的时候没有反应。搞了半天也不知道为什么。

换了一个软件，googleplay里面的，这次也推荐大家使用，比国内的文件管理好用多了。名字是x-plore，大家可以用用试试。

使用x-plore这个软件，直接 LAN-添加服务器-输入服务器ip-用户密码留空 保存 访问。不过费劲的问题是，虽然这个软件能看到局域网的共享，访问也是出现错误，然后按照常规的想法试了一些配置，重启软件，不过都不行。这里得到一个教训就是日志文件很重要，很多时候看看日志就知道了，而不是不断的查看网络的一些不靠谱的答案。

samba软件的日志文件位置为 `/var/log/samba/log`。日志的错误内容如下：

```bash
Bad SMB2 signature for message
this client does not support the negotiated dialect
```

然后解决的方法就是增加samba支持的协议，在配置文件中（/etc/samba/smb.conf)增加 `min protocol = LANMAN2` 配置。

没有苹果手机，所以就没测试ios。

参考资料：

- [SMB connection to Samba v4.11.6 totally fails](https://www.ghisler.ch/board/viewtopic.php?f=22&t=58420)

- [SMB1 is disabled by default](https://wiki.samba.org/index.php/Samba_4.11_Features_added/changed#SMB1_is_disabled_by_default)

- [Can't connect via LAN plugin](https://www.ghisler.ch/board/viewtopic.php?f=22&t=53707)
