---
title: 解决Mac系统Chrome Helper进程占用100%CPU
date: 2018-06-27
categories: Net
tags:
- Mac
- Chrome Helper
---



前段时间在用Mac的时候突然风扇狂转，一会温度就起来了。觉得很奇怪没有看视频，玩游戏，就是浏览网页，找到系统监视器查看，发现 `Google Chrome Helper` 的CPU占用率达到120%，谷歌以后找到解决方法。

解决方法如下：

[原文地址](https://apple.stackexchange.com/questions/272000/how-to-make-google-chrome-helper-not-use-as-much-cpu)

> Invoke ⋮ Chrome Menu → More Tools → Task Manager to see what exactly consumes CPU. Because the helper is a black box from the OS' side of view. I personally found a mining extension that way.

*一般是插件的问题，不过我遇到有一次居然是百度贴吧网页出现100%占用，找到占用资源的插件网页关掉即可。*

<!--more-->

**网络还有一种方法已经过时，我刚开始就是按照那种方法，把我的chrome改成英文的也没找到设置的地方，这里也写出来避免走弯路。**

> Chrome menu → “Preferences” → “Show advanced settings…” → “Privacy” → “Content Settings” → “Plug-ins” → “Click to play.”

[原文地址](https://www.wired.com/2014/10/google-chrome-helper/)

我当时查看的时候居然忘记看文章时间是 `2014` 年。
