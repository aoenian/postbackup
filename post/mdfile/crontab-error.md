---
title: 解决Ubuntu Server周期任务不执行问题
date: 2020-09-13 09:44:41
categories: Linux
tags:
- cron
- not work
---

想在树莓派中设定一个定时关机的命令，这样就不用每天晚上ssh进入关机了。不过晚上检查效果的时候发现没有执行。

## 查看日志

这次就知道日志的重要性了，但是ubuntu默认没有打开crontab的日志记录，所以我们首先要做的是打开cron日志记录功能。

<!--more-->

## 开启cron日志

找到cron的日志配置文件，位置为 `/etc/rsyslog.d/50-default.conf` 使用编辑器编辑。

把配置文件中的 `cron.*             /var/log/cron.log` 这一行代码注释掉，也就是让系统开启cron日志记录。

然后使用命令 `sudo service rsyslog restart`  重启系统日志

这时候就可以在 `/var/log/cron.log` 文件中查看周期命令的执行情况了。

## 出错原因

查看日志后知道出错的原因，这里建议一定使用命令的**绝对路径**，这样才能正确执行，如果需要每分钟都执行，那么直接使用五个*号即可。

修改完成后即可重启cron服务，`service cron restart`

参考资料：

- [如何用crontab每隔1分钟执行一个命令行脚本](https://segmentfault.com/q/1010000002516428)

- [ubuntu打开crontab日志及不执行常见原因](https://blog.csdn.net/panyox/article/details/79157046)
