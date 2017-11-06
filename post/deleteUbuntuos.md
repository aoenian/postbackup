---
title: 双系统删除Ubuntu（Win7+Ubuntu）
date: 2016-08-10 18:57:50
categories: Linux
tags:
- ubuntu
---

这个也是无奈之举，个人是很喜欢linux系统，不过ATI显卡确实不适合linux，本来机器就老了，散热也不行，加上显卡发热太大，开机不到一个小时就过热自动关机。所以没办法啦，只能删除Ubuntu。

**注意：不能直接删除分区，直接删除Win7就进不去了。因为Ubuntu在管理Grub**
<!-- more -->

**处理方法：**

+ 下载MbrFix.exe，修复MBR要用，可以google搜索一下。也可以在[这里](http://pan.baidu.com/s/1yBofk)下载。

    下载完成以后解压缩，将软件放在C盘（系统盘）。

    点击 开始——>运行——>输入cmd 回车 打开命令行界面

    进入MbrFix.exe所在的文件夹（在命令行界面），运行命令：

    `MbrFix /drive 0 fixmbr /yes ` 然后Enter回车

    此时MBR修复完成。

+ 为了保险，这时候重启操作系统，查看是否直接进入Win7.

    如果没问题，那么直接删除Ubuntu分区即可。

+ 删除Ubuntu分区：

    以Win7为例：

    开始-控制面板-管理工具-存储-磁盘管理 找到Ubuntu的分区删除，重新格式化为NTFS格式即可。

    *Ubuntu将烟消云散*
