---
title: 树莓派安装ffmpeg（主要用于you-get视频的合并）
date: 2016-12-01 15:38:33
categories: Raspberry Pi
tags:
- ffmpeg
- raspberry pi
- 安装
---

买了树莓派本来准备用来下BT，可谁知道国内除了迅雷，其他的下载软件的速度实在没办法看，所以平时下载大部分用you-get，还是挺好用的。

因为文档上面说过需要安装ffmpeg，不过我在树莓派上使用了几次you-get居然下载合并成功了，不过由于我没注意看是否是分段下载的，也没注意是哪个网站，所以基本可以忽略，大还是老老实实安装。

这也是我之前没有安装，可以昨天下载乐视出现问题了。无法合并，所以只能安装ffmpeg。

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
