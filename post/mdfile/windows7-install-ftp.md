---
title: Windows7安装FTP服务器
date: 2019-09-12
categories: Net
tags:
- Windows
- FTP 
---

记录Windows7安装FTP服务器，只是简单的安装使用，复杂的功能没有涉及到。

## 安装FTP

控制面板-程序和功能-打开和关闭windows功能-Internet信息服务-勾选 FTP服务和FTP扩展性 安装组件。

<!--more-->

## 新建并设定FTP服务器

控制面板-打开管理工具-iis服务器-新建ftp服务器  ftp站点名称 内容目录自行设定

**绑定IP地址** 如果选用全部为分配（所有满足的IP）那么本地访问的时候可以使用（本机的局域网ip或者用 127.0.0.1） 如果选择本机IP地址绑定则只能使用绑定的IP进行测试访问
自动启动ftp站点

虚拟主机名可以暂时不选，ssl可选择无

身份验证选择匿名则所有人可访问，授权方面则需要把允许访问调整为匿名用户，权限可以设置为读取。

参考文章：

- [windows服务器 IIS FTP服务](https://www.cnblogs.com/centos2017/p/7896688.html)
