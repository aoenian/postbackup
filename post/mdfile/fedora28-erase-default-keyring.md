---
title: Fedora28去掉Chrome/Opera浏览器的默认密钥环
date: 2018-07-18
categories: Linux
tags:
- fedora
- keyring
---

**安装浏览器的时候弹出的设置默认密钥，建议大家不要设置，不过既然你看到这篇文章估计已经晚了。**

那么解决方法如下：

打开终端 执行命令 `seahorse` 在弹出的窗口中右键点击默认密钥环，然后选择更改密码，输入之前设置的密码，新密码置空，确认即可。
