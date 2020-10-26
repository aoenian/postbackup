---
title: Manjaro安装IBUS输入法
date: 2019-05-15
categories: Linux
tags:
- ibus
- rime
---

经过一段时间的无聊装机，我又把Linux系统换回Manjaro来了，就是个这么没有原则的人，原因请看[这里](https://aoenian.github.io/2018/06/30/manjaro-linux-start-end/)。中间居然还萌生了要试试LFS的想法（~~太疯狂了，幸亏及时拉回来，避免了一场惨剧的发生~~)。

不过现在呢，既然换回Manjaro了，就准备慢慢的把常用的软件和配置过程记录下来，便于以后操作当然也希望刚接触这个系统的人少走一些弯路（_如果能做到那是个很好的事_）。

安装系统的事情简单的说一句，**我用rufus做的系统无法引导，后来改用了win32disk可以了。**

## 基本环境

- [manjaro](https://mirrors.ustc.edu.cn/manjaro-cd/) 选择的KDE桌面

<!--more-->

## 软件安装

### 更新系统

```bash
# 更换源
sudo pacman-mirrors -i -c China -m rank
sudo pacman -Syy        # 换源后更新缓存
sudo pacman -Syyu       # 更新系统
```

### 安装配置ibus-rime

由于还不熟练这个系统的命令，这里就用软件安装啦，见谅见谅

1. 打开 Octopi 安装 ibus-rime ibus-pinyin ibus-qt
2. 在你的 home 目录下的 **.xprofile** 文件中增加如下代码（没有请创建）

    ```bash
    export GTK_IM_MODULE=ibus
    export XMODIFIERS=@im=ibus
    export QT_IM_MODULE=ibus
    ibus-daemon -x -d
    ```

3. 注销系统（建议重启）应该就搞定了
4. 如果依然无法使用则需要执行 `qtconfig-qt4` 在 **Interface** -> **Default Input Method**，把默认的输入法引擎由 **xim** 更改为 **ibus**。

参考博文：

- [Arch 安装中文输入法](https://blog.csdn.net/Tangcuyuha/article/details/80297905)