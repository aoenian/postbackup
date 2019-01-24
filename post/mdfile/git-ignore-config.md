---
title: macOS下git忽略.DS_Store文件
date: 2018-12-19
categories: macOS
tags:
- .DS_Store
- gitignore
---

平时用git不是太多，一般就是保存一下配置文件和博文，所以有时候在提交的时候就把一些macOS的隐藏文件删除以后再跟踪提交。不过随着使用频率增加了一点点（主要是强迫自己每周写一些内容，不知能否坚持），然后觉得每次这样做就太浪费人力了。

然后就在项目中写了个`.gitignore`文件，不过这个文件管理的范围太小，只能管本层目录，子目录下面的就无能为力，而且每个项目都要有一个（之前不知道还有全部忽略这种玩意-_-!)，然后在网络上找到添加全局忽略的方法，具体操作如下：

<!--more-->

1. 在你的`home`目录下面建立`.gitignore_global`文件，内容如下：
    ```bash
    # .gitignore_global

    .DS_Store
    .DS_Store?
    *.swp
    ._*
    .Spotlight-V100
    .Trashes
    ```

    _如果大家需要忽略更多的文件，可以直接写入即可。_

2. 然后在你的`home`目录下的`.gitconfig`文件中引入上面的`.igtignore_global`文件。

    **注意：** 如果目录下面找不到`.gitconfig`配置文件，可能你还没有配置git，可以执行如下命令

    ```bash
    git config --global user.name "xxx"    # xxx 是你的用户名
    git config --global user.email "xxx@xx"  # xxx@xx 是你的邮箱地址
    ```

3. 在`.gitconfig`里面加入内容如下：

    ```bash
    [user]
        name = aoenian
        email = aoenian@zoho.com.cn
    [core]
        excludesfile = /Users/aoenian/.gitignore_global
    ```

    _里面的home目录的名字要替换成你自己的_

参考博客：

- https://www.jianshu.com/p/9386124cb5b5