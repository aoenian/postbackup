---
title: 树莓派安装ffmpeg（主要用于you-get下载视频的合并）
date: 2016-12-01 15:38:33
categories: Raspberry Pi
tags:
- ffmpeg
- RPI
---

买了树莓派本来准备用来下BT，可谁知道国内除了迅雷，其他的下载软件的速度实在没办法看，所以平时下载大部分用you-get，还是挺好用的。

因为文档上面说过需要安装ffmpeg，不过开始我没有安装使用you-get也下载合并成功了，但是我没注意看是否分段下载的，也没注意是哪个网站。昨天下载乐视出现问题了，无法合并，所以只能安装ffmpeg。

**2018-06-17更新：由于长时间没有关注you-get项目，这段时间感觉很多网站已经无法下载，不过自己看视频也少了，ffpmeg可以直接用apt-get安装不过可能版本会低一点，一般不影响。**

**Windows使用ffmpeg可以[官网](https://ffmpeg.zeranoe.com/builds/)下载Static或Shared版本**

**具体的教程还是移步到[雷霄骅的博客](https://blog.csdn.net/leixiaohua1020/article/details/15811977)**

具体的安装方法大家可以参考以下两个博客：

+ https://alair.cn/logs/raspberry-pi-install-ffmpeg.html

+ http://owenashurst.com/?p=242

<!-- more -->

因为我是先按照第二个博客中的步骤操作，然后感觉太多又换到第一个博客。所以我也不清楚具体需要安装哪些，这就尴尬了-_-!

不过我还是把我安装的步骤写下来以供参考：

```bash
# 先删除已经存在的依赖，然后更新系统重新安装依赖包
sudo apt-get remove --purge libmp3lame-dev libtool libssl-dev libaacplus-* libx264 libvpx librtmp ffmpeg

sudo apt-get update; sudo apt-get upgrade; sudo apt-get install libmp3lame-dev; sudo apt-get install -y libopus-dev; sudo apt-get install autoconf; sudo apt-get install libtool; sudo apt-get install checkinstall; sudo apt-get install libssl-dev

# 然后按照上面第一个博客的步骤
# 安装h264编码器
mkdir ~/src
cd ~/src
git clone git://git.videolan.org/x264
cd x264
./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl
make
sudo make install

# install ffmpeg
cd /usr/src
git clone git://source.ffmpeg.org/ffmpeg.git
cd ffmpeg
sudo ./configure --arch=armel --target-os=linux --enable-gpl --enable-libx264 --enable-nonfree
make # make 时间稍微长点，可以后台运行  nohup make &
sudo make install
```
