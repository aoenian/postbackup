---
title: Next主题增加Gitment评论系统
date: 2019-04-14
categories: Hexo
tags:
- next
- 评论
- gitment
---

2019-06-30更新：`object ProgressEvent`错误再次出现，更新可用代码，具体可查看文章最后。

刚开始建立博客的时候用了评论系统，由于当时觉得国内的注册太麻烦，就用了**DISQUS**，不过国内一直无法访问，不知道为啥。后来重装系统后再次建立博客时，就把评论关掉了。

前段时间突然看到了gitment（用github的issue做博客评论系统），觉得这个还不错，就准备试试。然后就有了这篇文章。

<!-- more -->

## 准备工作

- 一个github帐号（~~当我没说~~)

- 用你做博客的github帐号注册一个**OAuth application**

    注册地址点[这里](https://github.com/settings/applications/new)，填写内容如下图：

    ![oauth](https://raw.githubusercontent.com/aoenian/blog-images/master/2019041401.png)

    **注意：** Authorization callback URL用你的博客的域名，其他的可以随便填写。点击注册按钮后会出现`Client ID`和`Client Secret`记录下来，等会会用到。

- （此步骤可选）新建一个repo，用这个repo的issue来存储博客

    我建立了一个[blog-comments](https://github.com/aoenian/blog-comments)来存储。

## 修改主题配置文件

修改next主题的配置文件_config.yml，找到 gitment 选项修改如下：

```yml
gitment:
  enable: true
  mint: false # 建议false
  count: true # Show comments count in post meta area
  lazy: false # Comments lazy loading with a button
  cleanly: false # Hide 'Powered by ...' on footer, and more
  language: # Force language, or auto switch by theme
  github_user: aoenian # MUST HAVE, Your Github Username
  github_repo: blog-comments # 你存储评论的repo 如果没有新建可以是 你的博客repo 如：aoenian.github.io
  client_id: '你的Client ID' # MUST HAVE, Github client id for the Gitment
  client_secret: '你的Client Secret' # EITHER this or proxy_gateway, Github access secret token for the Gitment
  proxy_gateway: # Address of api proxy, See: https://github.com/aimingoo/intersect
  redirect_protocol: # Protocol of redirect_uri with force_redirect_protocol when mint enabled
```

正常情况，到这里就完成了，接下来就是执行`hexo clean && hexo g && hexo d`然后在评论区会显示**未开放评论**，这时用自己的帐号点击登入，然后验证即可手动开放（每篇文章需要这样做一遍）。

但是我在输入用户名密码登录后，页面出现错误提示：**object ProgressEvent**，评论登录一直转圈圈，具体原因可查看 <https://github.com/imsun/gitment/issues/170>

## object ProgressEvent问题解决

找到next主题中的gitment评论文件 **next/layout/_third-party/comments/gitment.swig**，更改以下代码

```html
  <!-- 原代码 -->
  <link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
  <script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>

  <!-- 更改后 此代码已经无法使用-->
  <link rel="stylesheet" href="https://www.wenjunjiang.win/css/gitment.css">
  <script src="https://www.wenjunjiang.win/js/gitment.js"></script>

  <!-- 请使用下面的替换  2019-06-30更新-->
  <link rel="stylesheet" href="https://billts.site/extra_css/gitment.css">
  <script src="https://billts.site/js/gitment.js"></script>
```

重新清理缓存，部署博客即可留言。

参考博文：

- [Hexo博客主题NexT相关配置](https://huangjunjia.github.io/2019/02/27/blog/blog-theme-set/)

- [hexo添加gitment评论系统](http://kuring.me/post/gitment/)

- [添加Gitment评论系统踩过的坑](http://xichen.pub/2018/01/31/2018-01-31-gitment/)

- [GitHub Pages个人博客搭建流程](https://adamhu.github.io/2019/06/GitHub-Pages%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E6%B5%81%E7%A8%8B/) 2019-06-30更新代码出处博客
