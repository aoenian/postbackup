---
title: GithubPages和Hexo建立个人博客
date: 2016-08-08 12:50:39
categories: Net
tags:
- github pages
- hexo
---

之前一直用现成的博客网站写博客（其实就是记性不好），后来感觉限制太多，而且编辑方式也不是很方便（没有markdown）。也想到建一个自己的网站，不过太麻烦（其实不会弄）。后来发现了githubpages，良心产品，所以就整了一个。

不说废话，第一篇博客还是记录一下自己的配置过程。

<!-- more -->

环境配置
========

需要安装的软件
--------------

-   brew (Mac，Win下不需要)
-   nodejs
-   git
-   hexo

<!--more-->

安装方法
--------

*建议查看官方网站获取最新的安装建议,这里只记录Mac系统（版本10.13.4）的方法,Windows 可直接在官网下载相应的安装包安装*

**由于一些原因，`npm`命令安装模块很慢，如果需要可以使用[淘宝npm镜像](http://npm.taobao.org/)**

-   [brew](https://brew.sh/index_zh-cn) Mac系统下的包管理器

    ``` Shell
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  
    ```

-   [nodejs](https://nodejs.org/en/) `brew install node`
-   [git](https://git-scm.com/) `brew install git`
-   [hexo](https://hexo.io/zh-cn/docs/index.html) `npm install -g hexo-cli`

hexo建站
========

参考官方文档 [建站](https://hexo.io/zh-cn/docs/setup.html)

方法如下：

``` Shell
$ mkdir ghblog
$ cd ghblog
$ hexo init
$ npm install
$ hexo s    # 启动服务器
```

访问 http://localhost:4000/ 若出现 Hexo 的博客则建站成功。

网站配置
========

站点配置
--------

站点配置请参考官方文档和主题文档 [配置](https://hexo.io/zh-cn/docs/configuration.html)

**注意：配置文件中配置项目的 `:` 后要空一格**

安装theme
---------

建议大家使用`Next`主题，毕竟博客还是内容重要，等后面大家熟悉以后可以自行创建自己的主题。

`Next`主题的配置在[这里](http://theme-next.iissnan.com/getting-started.html)

配置完成后可以执行 `hexo s` 命令，访问 http://localhost:4000/ 是否起作用。


Github配置
========

Pages项目建立
----------

- 申请一个Github帐号，并进入
- 点击页面你的帐号旁边的`+`号，点击`New repository`新建一个仓库
- 仓库名字为`yourname.github.io`
    - yourname就是你的账户名
    - Decription是仓库的描述可以随便填
    - 仓库的性质选择`Public`
    - 勾选 `Initialize this repository with a README`
    - `Add .gitignore` 和 `Add a licence` 可以不用选
- 最后点击 `Cteate repository` 即可，现在不用再进行额外的设置了
- 访问 https://yourname.github.io/ 查看Pages是否成功建立 

hexo站点部署配置
----------

本地站点完成，Github Pages项目也建立，剩下的就是把两者联系起来，需要修改站点配置文件中的 `deploy` 选项中内容，具体如下(Windows下相同)：

    deploy:
      type: git
      repository: https://github.com/aoenian/aoenian.github.io.git
      branch: master

若部署时出错，可查看 [部署](https://hexo.io/zh-cn/docs/deployment.html)文档，解决方法是在博客项目目录下安装 `hexo-deployer-git` 执行命令如下：
`npm install hexo-deployer-git --save`

**文章的撰写，站点的具体配置，主题的具体配置，请查看相应的官方文档**


**参考链接**

+ [Mac上搭建基于GitHub的Hexo博客](http://gonghonglou.com/2016/02/03/firstblog/)



