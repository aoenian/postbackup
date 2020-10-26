---
title: pip换源提升包下载速度
date: 2019-09-26
categories: Python
tags:
- change
- repositories
---

快到国庆节了看到公众号有一篇文章是使用python给自己的头像添加国旗。然后感觉挺厉害，可以用几行代码来把图片合成起来。在使用过程中遇到了一些问题，记录如下：

## 给头像添加国旗

<!--more-->

具体的文章链接在参考文章中，文中代码摘录如下：

```Python
# -*- coding: utf8 -*-
import cv2
# 读取头像和国旗图案
img_head = cv2.imread('head.jpg')
img_flag = cv2.imread('flag.png')
# 获取头像和国旗图案宽度
w_head, h_head = img_head.shape[:2]
w_flag, h_flag = img_flag.shape[:2]
# 计算图案缩放比例
scale = w_head / w_flag / 4
# 缩放图案
img_flag = cv2.resize(img_flag, (0, 0), fx=scale, fy=scale)
# 获取缩放后新宽度
w_flag, h_flag = img_flag.shape[:2]
# 按3个通道合并图片
for c in range(0, 3):
    img_head[w_head - w_flag:, h_head - h_flag:, c] = img_flag[:, :, c]
# 保存最终结果
cv2.imwrite('new_head.jpg', img_head)
```

因为这里用了**python-opencv**库来处理图像，所以我们需要先安装。

## 替换pip源

之前一直没有想过pip可以换源，然后安装第三方库的时候需要执行好几次才能成功，而且速度非常慢。这次的速度更慢，实在忍受不了所以就搜索了一下解决方法，没想到就更Linux的软件源一样换成国内的就可以了。

看来下次遇到这种情况应该先想到换源的方法。

具体操作很简单，执行如下代码即可：

```bash
# windows在CMD中，Linux Mac在终端执行，这个换的是阿里的源
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

# 更新pip命令
python -m pip install --upgrade pip
```

## 安装python-opencv库

```bash
# 使用的是python3
pip3 install numpy
pip3 install opencv-python
```

参考文章：

- [别再@官方啦，普天同庆加国旗，10行代码搞定](https://mp.weixin.qq.com/s/9OSmAsQWZsTu8nBfCkMLDA)
- [pip下载python库太慢怎么办？](https://zhuanlan.zhihu.com/p/46975553)
- [python3之opencv安装](https://blog.csdn.net/qq_25005909/article/details/78554469)
