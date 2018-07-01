---
title: Rime输入法配置——小鹤双拼
date: 2016-11-26 11:00:45
categories: Tools 
tags:
- Rime Input
- flypy
---

以前一直在用搜狗，不过随着搜狗输入法离输入这条路越来越远，导致不得不寻找一款替代品，然后找到了QQ输入法，其实腾讯的产品还是不错的，起码输入法不会整天提示你系统垃圾太多。

**重磅消息：时隔四年，小狼毫更新了，适配Windows10，哈哈**  [下载地址](https://rime.im/download/)

四月份的时候windows上面的小狼毫收到更新通知还以为是幻觉-_-!

<!-- more -->

但是在mac下面腾讯的输入法表现的就差强人意了。所以又踏上了寻找的路。无意中发现了佛振开发的Rime输入法，仔细了解以后发现这才是真正的输入法(大部分输入法都不干正事)。输入快速，而且全平台通用，绝对是使用多个平台的用户福音。

当然缺点（优点？）就是配置问题，因为输入法需要高度定制，所以没有太多的图形化配置界面，不过我觉得能找到这个输入法的估计配置都没问题。-_-

**还有一个问题，我没有配置任何词库，之前配置过，一个是麻烦，另一个是用处并不大，对于我来说。词库要靠自己养啊。**

官网：http://rime.im/

参考博客：http://www.dreamxu.com/install-config-squirrel/

这里建议大家查看上面的博客，他里面的配置更加详细，同时有词库的配置。所以具体的配置我就不写了，因为我的配置不涉及词库很简单。具体配置文件在[这里](https://github.com/aoenian/rime-config) 如果有需要可以直接下载使用。 

*Ubuntu 系统需要注意，如果使用双拼不仅需要安装rime，还需要安装双拼方案*

具体执行如下命令：

```bash
sudo apt-get install ibus-rime # 安装 ibus-rime 
sudo apt-get insatll librime-data-double-pinyin # 安装双拼方案 
```

具体可以参考： https://www.v2ex.com/t/232790  

> 日志文件的位置记录下，总是忘掉
>【中州韻】 /tmp/rime.ibus.*
>【小狼毫】 %TEMP%\rime.weasel.*
>【鼠鬚管】 $TMPDIR/rime.squirrel.*
> 各發行版的早期版本 用戶資料夾/rime.log


