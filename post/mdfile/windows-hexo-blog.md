---
title: Windows7+Github+Hexo搭建个人博客
date: 2019-04-24
categories: Hexo
tags:
- windows
- git
---

## 准备工作

1. github帐号一枚

    注册github，输入用户名，邮箱，密码即可注册。然后新建一个repo，命名为`username.github.io`其中username替换为你的用户名。建议勾选**Initialize this repository with a README**`。

    仓库建立完成后，即可访问 <https://username.github.io> 能够看到一个白色页面，写着你的仓库名及仓库描述（*如果你建立仓库的时候写了的话*）

    <!-- more -->

2. 下载安装node

    下载地址：[node](https://nodejs.org/en/download/) 建议使用LTS版本，安装完成后打开CMD执行命令 `node -v` 返回相应的版本号则安装成功。

3. 下载安装git

    下载地址：[git](https://git-scm.com/downloads) 安装完成后在CMD中执行命令 `git --version` 返回相应的版本则安装成功。

## 安装hexo

1. 安装淘宝npm镜像

    由于网络环境的原因，建议更换淘宝npm镜像，具体可查看[淘宝 NPM 镜像](http://npm.taobao.org/)。

    在cmd中执行如下命令进行更换：

    `npm install -g cnpm --registry=https://registry.npm.taobao.org`

    使用方法：更换完成后，即可用`cnpm`替换后面命令中的`npm`。

2. 安装hexo

    具体可参照官方网站[Hexo](https://hexo.io/zh-cn/)。

    这里介绍如下：

    ```bash
    # 在cmd中执行此命令安装hexo
    # 如使用淘宝镜像，则记得替换为cnpm
    npm install hexo-cli -g

    # 初始化博客命令，会建立一个blog的目录
    # blog 只是目录名，可以修改
    # 可能出现警告 Failed to install dependencies
    # 不用担心警告继续向下执行即可
    hexo init blog

    # 进入blog目录
    cd blog

    # 依赖安装，可以替换为cnpm
    npm install

    # 启动hexo
    hexo server
    ```

    启动hexo以后，在命令行会出现：**Hexo is running at <http://localhost:4000>** 这个时候就可以用浏览器访问这个地址，到此本地的博客框架已经搭建完成。

    注：`CTRL+C` 是停止本地的博客服务。

## 部署

1. 安装 hexo-deployer-git

    在blog目录中执行 `cnpm install hexo-deployer-git --save`

2. 修改hexo配置文件

    hexo的配置文件位置：**blog/_config.yml** 建议使用vscode、nodepad++等编辑器打开，不要使用Windows记事本。

    具体的配置信息请查看官方网站：[配置](https://hexo.io/zh-cn/docs/configuration) 这里只简单介绍github的部署配置

    打开_config.yml文件，然后找到文件最后的 `# Deployment` 这一段，更改如下：

    ```yml
    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repo: <repository url> #你的仓库的url 如：https://github.com/aoenian/aoenian.github.io.git
      branch: master
    ```

完成后即可在CMD窗口中切换到blog目录下执行命令`hexo clean && hexo g && hexo d`，输入用户名和密码(密码在终端不会显示)即可部署成功。