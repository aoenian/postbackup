---
title: MacOS/RPI 终端使用ss代理
date: 2017-02-18
categories: Raspberry Pi
tags:
- ss
- terminal
---

这段时间需要编写程序访问一个网站，不过出现一个问题就是，网站无法在国内正常的访问，我是真的崩溃了。vim下载个插件，网络出问题，emacs整个配置也是问题百出。哎，无语凝噎！！

所以咱们不能坐以待毙，终端使用ss走起。

<!--more-->

**测试环境：**

-   操作系统：RaspberryPi MacOS

安装ss
------

`pip install shadowsocks`

编辑ss的配置文件，位置 `/etc/shadowsocks.json` ，填写代码如下：

**注意：/etc目录下面没有配置文件无所谓，在home下面找个地方随便建一个就可以**

``` bash
{
    "server":"your_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"yourpassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

**配置文件中的 `method` 需要注意，如果需要类似 `chacha20` 的加密方式，那么请看 [这里](https://ls12.me/libsodium-install.html)，不过我没有尝试。**

运行ss
------

启动： `sslocal -c /etc/shadowsocks.json`

停止直接快捷键 `ctrl-c` 这里我没有选择后台运行主要是为了查看访问网站时候的出错信息

如果需要后台运行可以使用如下命令：

``` bash
sslocal -c /etc/shadowsocks.json -d start    # 启动
sslocal -c /etc/shadowsocks.json -d stop    # 停止
```

使用终端代理
------------

这里主要是由于ss使用的socks5协议终端不支持，所以需要使用协议转换工具。

原文的 `polipo` 我这里没有配置成功，所以使用另一个软件 `proxychains`

-   安装proxychains

``` bash
# Debian/Ubuntu:
apt-get install proxychains
# Mac OS X:
brew install proxychains-ng
```

-   配置文件

编辑 `~/.proxychains/proxychains.conf` 如果没有直接建一个

``` bash
strict_chain
proxy_dns 
remote_dns_subnet 224
tcp_read_time_out 15000
tcp_connect_time_out 8000
localnet 127.0.0.0/255.0.0.0
quiet_mode

[ProxyList]
socks5  127.0.0.1 1080
```

-   使用方法

    -   通过proxychains运行命令：

        `proxychains4 curl https://www.twitter.com`

        `proxychains4 git push origin master`

    -   直接代理bash（个人推荐，省心省力）

        `proxychains4 bash`

    -   测试

        `curl ip.gs` 可以查看你现在的ip地址 来检测现在是否代理成功

**参考文章链接如下：**

-   <http://www.jianshu.com/p/8e7d7f57bf59>
-   <https://segmentfault.com/a/1190000002589135>
