---
title: 用U盘安装macOS High Sierra
date: 2019-03-16
categories: macOS
tags:
- Sierra
- install
---

手边有一台10年的macbookpro，配置还行，不过由于安装了windows系统，驱动可能有问题，一直没有声音，触摸板也很奇怪，然后准备重新装回macOS。

# 安装方式

## 联网自动安装

这个是最先想到的方法，因为现在用的mba就是这样重装的，只要连上网络，干会儿其他的事情，等会回来崭新的系统就在迎接你了，不得不说苹果公司在软硬件的结合方面真的很厉害。让人越用越懒，用了以后基本就没再手动装过系统了。

**但是重点来了**，由于笔记本的年龄太大了，直接开机`Command+R`找到重新安装macOS，谁知道自动恢复的系统是`OS X Mountain Lion`，直接提示**已经下架**，网络上找了解决的方法，说是中国区下架了，把appleID更改为国外即可，_经测试更改为美国依然无法自动安装。_

<!--more-->

这条路堵死了。这里是官方的文档：[关于 macOS 恢复功能](https://support.apple.com/zh-cn/HT201314)

## 手动U盘安装

参考官方文档：[如何创建可引导的 macOS 安装器](https://support.apple.com/zh-cn/HT201372)

这里需要注意，制作启动U盘需要另外一个可以启动的mac电脑。

具体操作如下：

1. 下载macOS，其实就是在appstore里面下载相应的操作系统，建议使用新的系统。[macOS Mojave](https://itunes.apple.com/cn/app/macos-mojave/id1398502828?mt=12) 和 [macOS High Sierra](https://itunes.apple.com/cn/app/macos-high-sierra/id1246284741?ls=1&mt=12) 下载完成后会出现在系统的应用程序里面类似 `Install macOS Mojave`，这个就是安装器，如果安装器自动打开，退出即可无须安装。

2. 插上U盘（数据自行备份，等会制作的时候就没了），找到**磁盘管理**程序，打开以后找到U盘，点击抹去，格式选择**Mac OS 扩展格式**，并把U盘驱动器的卷宗名称设定为`MyVolume`。

3. 打开**终端**，然后根据你下载的系统，执行如下命令：

    ```bash
    # Mojave:
    sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

    # High Sierra:
    sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

    # Sierra
    sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ macOS\ Sierra.app
    ```

    回车后输入管理员密码，然后再次回车，输入`Y`确认制作启动盘，然后就是等待完成。

4. 把制作好的U盘插入待安装的mac上，然后重启电脑，立即按着`Option`进入启动磁盘选择的界面，选择你的U盘安装器，然后回车启动。

5. 这时会进入macOS恢复模式，直接选择**安装macOS**即可安装系统。这里建议先不用连接互联网，直接安装即可。