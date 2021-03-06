---
title: 老台式安装Windows10的问题解决
date: 2019-02-13
categories: Windows
tags:
- install
- error
---

过年回家准备把家里面的台式机重装系统，想想就直接Windows10了，省得以后麻烦。

谁知道都是老司机了，居然翻车了，轻车熟路通过u盘启动微pe系统，然后把Windows10的镜像加载到虚拟光驱，双击打开，启动`setup.exe`。嘣，弹出对话框提示：**使用其他标明64位的安装光盘...**，当时蒙了，这什么情况，32位的系统还装不了？于是进行了以下的操作。

<!--more-->

直达解决方法请点击[这里](#anchor).

# 尝试过的方法

1. 首先怀疑系统镜像问题，然后通过校验软件核对了[i tell you](https://msdn.itellyou.cn/)的SHA1，没有问题

2. 然后怀疑Windows10的`consumer_edition`这个版本有问题，查了一下也是包含pro版本的。

3. 然后网络搜索解决方法，说是启动光盘中`sources`目录下面的setup.exe，不过双击后提示缺少dll文件，这个目录下的其他包含setup的都点击了一遍，失败。

4. 突然想到难道只有Windows10是这个问题，虚拟机加载Win7的镜像，双击setup成功出现安装界面，这个真的说不过去，系统都是越更新越难用吗

5. 既然系统的默认安装无法使用，那么就用pe里面的安装软件，WinNTSetup这个软件，选择相应的安装镜像，引导驱动器和安装驱动器都选择了**C:**盘，点击安装等待进度条完成，提示完成、重启，出现了windows10的图标，想着总算没问题了，结果就是一直的等待，半个小时，只能强制按电源关机。

6. 因为电脑只有**2G的内存**跑64位也不是太现实，所以就没有测试64位系统。

7. 安装了Win7，然后换了pe，准备死磕到底，不过按照之前的方法重新试了一遍依然失败。

当进行到这里的时候，我已经准备用Win7了，实在是**心力憔悴**。就在最后的时刻成功了。拯救我的方案在下面。哈哈！！柳暗花明又一村啊。。。

<span id="anchor"></span>

# 解决方案

这里推荐两个PE系统，一个是之前在用的[微PE](http://www.wepe.com.cn/)，另一个是这次安装Win10的[优启通](https://www.itsk.com/thread-392998-1-1.html)。

解决很简单，启动优启通pe的Windows10系统，然后点击`EIX系统安装`程序然后出现恢复镜像，选择Windows 10 Pro，目标分区选择C盘，点击一键恢复，等待安装即可。

具体为什么成功，我也不知道，就是这么**诚实**。