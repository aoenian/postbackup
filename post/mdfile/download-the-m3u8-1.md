---
title: 记一次视频下载过程1
date: 2019-08-31
categories: Python
tags:
- m3u8
- requests
---

在一个网站看视频的时候，感觉视频缓冲太慢，而且快进容易被卡死。然后就想把视频下载下来。还是和以前一样，通过开发者工具看了看视频传输的数据。原以为会找到一个mp4文件直接下载，谁知道全是一段一段的ts文件，这是什么时候出现的变态技术，我也是多长时间没有下载视频了。

更变态的是ts文件只有几兆大小，而且下载下来以后还无法观看，后面知道因为加密的原因。**这里先说结论：我没有搞定ts的解密**。所以这篇文章可能没有你想看的。

<!-- more-->

这里记录在下载视频过程中遇到的一些问题。

## python获得网址状态

需求是为了查看网址是否能够访问。方法摘自 <https://stackoverflow.com/a/13641613>

具体如下：

1. 首先你要安装 `requests` 包，当然后面要用的。

2. 代码实现如下

    ```python
    import requests
    try:
        r = requests.head("https://stackoverflow.com")
        print(r.status_code)
        # prints the int of the status code. Find more at httpstatusrappers.com :)
    except requests.ConnectionError:
        print("failed to connect")
    ```

其他具体的用法可查看 [requests library](https://2.python-requests.org//en/latest/)

## python捕获所有的异常

具体查看 [捕获所有异常](https://python3-cookbook.readthedocs.io/zh_CN/latest/c14/p07_catching_all_exceptions.html)

代码如下：

```python
# 下面的代码会捕获除了 SystemExit 、 KeyboardInterrupt 和 GeneratorExit 之外的所有异常
try:
   ...
except Exception as e:
   ...
   log('Reason:', e)       # Important!

# 如果你还想捕获这三个异常，将 Exception 改成 BaseException 即可。
```

## 命令行下载

因为视频被切分几千段，然后我下载的时候用的是树莓派，所以要使用命令行下载。刚开始用的`wget`，后来感觉`aria2c`也是挺不错的，当然链接都是https的。

- wget下载

    wget使用命令 `wget -i url.txt` 来下载多个链接。开始我就是使用这种方式，但是下载速度很慢，只能一个一个下载，不能同时进行多个。

    然后在网络上寻找一次下载多个的方法，具体的问答可以查看 [Parallel downloading](https://stackoverflow.com/a/25064491)

    这里我只记录一个比较简单的方法：`cat urls.txt | xargs -n 1 -P 5 wget` 或者 `cat urls.txt | parallel -j 5 wget` 这里面的选项 `-P` 和 `-j` 后面的5是你想同时进行几个任务。

- aria2c

    这个就比较简单了，先安装 `sudo apt install aria2c` 然后使用 `aria2c -i url.txt`即可。

    具体可参考 [aria2c library](sudo apt install aria2c)

## 部署博客时出现xcrun: error

今天这篇博客的时候出现了`xcrun: error: missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun`的错误，搜索以后发现是更新系统引起的。这个错误在执行`git --version`也会出现。

解决方法：命令行执行 `xcode-select --install` 然后重新安装xcode command line。

参考博客：[解决MacOS升级后出现xcrun: error: invalid active developer path, missing xcrun的问题](https://www.jianshu.com/p/50b6771eb853)
