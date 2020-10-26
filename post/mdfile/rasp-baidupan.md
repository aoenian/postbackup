---
title: 树莓派下载百度云盘文件
date: 2020-05-10 16:09:52
categories: Raspberry Pi
tags:
- pan
- bypy
---

树莓派又闲置了一段时间，然后现在再次学习linux，这次不能一知半解，要学进去，把之前的一些盲点给补充起来。然后就装了ubuntuserver，用着挺不错。

然后前段时间pandownload的作者被抓了，原因大家都知道，不过因为百度依然是国内最受欢迎的云盘，不花钱也只能忍受这个速度，然而呢总是用电脑下载的话太费电了。就想起来使用树莓派，网络一搜果然已经有大神把轮子造好了，直接使用就ok。下面简单记录配置过程。

<!--more-->

## 项目选择

在搜索中发现了两个项目，项目地址和使用测试如下

- <https://github.com/liuzhuoling2011/baidupcs-web> 这个是web界面好操作，别看学linux，我也不想麻烦，鼠标点点就行也懒得整别的。**不过致命的问题是无法登录，这个好像作者一直没有解决，咱们能力有限，只能另寻他法**
- <https://github.com/houtianze/bypy> 使用命令行界面，基于python编写，功能全面，就他了。（看来命令行还是比较好处理，个人观点）

## 环境配置

- 设置utf8，按照官方给出的链接，我设置了en_US.utf-8 不过系统依然还是显示 C.utf-8，后面证实不影响使用，感觉应该是 UTF-8 即可

- 安装pip3, 执行如下命令`sudo apt install python3-pip`

- 安装bypy，执行如下命令`sudo pip3 install bypy` 这里如果用普通用户权限安装，完成后找不到命令。

整个配置过程没有出现问题，直接无脑执行命令安装即可，什么时候能够像这种高手一样开发出这么厉害的程序。

## 网盘配置

- 绑定网盘。

    执行命令`bypy info`，根据提示访问给出的网页，然后输入网页中出现的百度授权码回车即可。

- 把需要下载的文件放在 我的应用数据-bypy中即可下载

- 常用命令

    ```bash
    bypy help <command>     # 帮助
    bypy list               # 列出文件列表
    bypy syncup   或   bypy upload          # 上传到云盘
    bypy syncdown  或  bypy downdir /       #  下载到本地
    bypy compare        # 比较当前目录和云盘
    ```
