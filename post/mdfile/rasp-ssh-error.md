---
title: SSH登录树莓派出现login warning
date: 2016-10-14 11:10:08
categories: Raspberry Pi
tags: 
- login warning
---

写这个博文的时候已经离我遇到这个问题时间比较长了，我记得遇到这个问题的原因是我之前在笔记本上面使用SSH连接过树莓派，然后由于一些原因，我把树莓派重装了一次，接着再次用SSH登录的时候就出现问题了。

> SSH login warning: remote host identification has changed

<!-- more -->

不过是在MAC OS 下面出现的问题，其他的操作系统就不清楚了。不过感觉解决方法都差不多。

解决方法：

```bash
# 删除所有关于树莓派的key
ssh-keygen -R [your domain name or IP address]
```

下次直接连接以后会出现询问是否继续连接，直接输入 `yes` 即可。
