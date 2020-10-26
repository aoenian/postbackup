---
title: 日常使用中一些小问题汇总(一)
date: 2019-04-03
categories: Collection
tags:
- you-get
- java
- virtual box
- vscode
---

## 安装Eclipse

直接在Eclipse的 [官网](http://www.eclipse.org/downloads/packages/) 下载**Eclipse IDE for Java Developers**这个版本即可。

然后双击安装，启动后系统会提示安装 **旧javaSE6运行环境才能打开** 点击 **更多信息**，下载安装 **Java for OS X 2017-001**。

<!--more-->

## 树莓派安装you-get

平时在线看视频比较少，有时还是喜欢下载下来，树莓派可以担此重任，那就要用到[you-get](https://github.com/soimort/you-get)。不过安装之前需要处理依赖：

- Python 3.2 or above
- FFmpeg 1.0 or above
- (Optional) RTMPDump

执行如下命令解决依赖问题

```shell
# python3 树莓派已经安装
# ffmpeg 安装 (如果视频无需连接也可不安装，如有一些b站的视频)
sudo apt install ffmpeg

# 安装pip3
sudo apt install python3-pip

# 安装you-get
sudo pip3 install you-get
```

完成后即可使用`you-get`命令。

## 安装虚拟机virtualbox

在macOS之前的版本安装虚拟机（virtualbox）并没有出现这种情况，后来mac系统更新后出现的。

主要的问题就是安装过程失败，在最后一步安装失败，提示严重错误，这个主要是

> High Sierra comes with a new security feature: Secure Kernel Extension Loading, which blocks kernel extension loading.

解决方法：进入 **Preferences > Security & Privacy > General** 手动允许。

## vscode文本乱码

在windows系统下面下载了一些文本文件，拷贝到mac以后在vscode中打开出现乱码，选择文件编码依然没有效果，但是emacs打开没有问题。

解决方法如下：**将vscode设置中的"files.autoGuessEncoding"项的值改为true即可。**

参考链接：

- [raspberry-doesnt-find-pip3](https://stackoverflow.com/questions/53195375/raspberry-doesnt-find-pip3)

- [macOS 10.13 安装Virtualbox失败](https://www.jianshu.com/p/dc021a7196cd)

- [VS Code 中文注释显示乱码](https://www.zhihu.com/question/34415763/answer/162916517)