---
title: 更新node后hexo填坑记录
date: 2020-07-27 17:59:01
categories: Hexo
tags:
- typeError
- proxychains
---

起因是使用brew更新了系统的软件，然后问题就接二连三的出现了，这里记录一下填坑过程。

## 更换brew源

使用brew安装软件的时候总是一直卡在更新，原因大家都知道，所以就想着把源更换了。

用了中科大的源，地址如下 [替换及重置Homebrew默认源](https://lug.ustc.edu.cn/wiki/mirrors/help/brew.git) [Homebrew Bottles源](https://lug.ustc.edu.cn/wiki/mirrors/help/homebrew-bottles)

不想跳转的可以看下面的摘录：

<!--more-->

```bash
# 替换及重置Homebrew默认源
# 替换brew.git:
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
# 替换homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# 却换回官方源
# 重置brew.git:
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
# 重置homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git

# Homebrew Bottles源
# bash用户
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
# zsh用户
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc

# Homebrew Bottles源切换回官方
# 注释掉bash配置文件里的有关Homebrew Bottles即可
# 重启bash或让bash重读配置文件
```

## hexo s启动服务器出错

切换了brew的源以后，测试了更新，速度快多了，但是没等更新完成我就直接中断了。我怀疑就是这个问题，然后我在启动服务器时出现错误，类似下面这种，还提示node什么的。

```bash
TypeError [ERR_INVALID_ARG_TYPE]: The "mode" argument must be integer. Received an instance of Object
```

然后我就怀疑可能是node出问题，我尝试了执行 `node` 命令查看版本，发现命令执行失败了，然后确定了就是更新的时候中断造成。

### 更新node

更新了brew源以后直接更新了node，但是提示node已经安装且为最新，看来使用更新不行了，重装一遍吧

执行命令 `brew intall node` ，安装完成测试了node版本也正常输出，这次应该没事了吧

然后再次执行依然出问题，这次搜索了以后发现可能是hexo的版本太老配不上node14，因为安装hexo后一直就没更新过，所以用的还是3版本，然后更新呗。

### 更新hexo

没想到的是npm更新包其实挺麻烦和正常的思路不是太一样，我直接使用 `npm update` 并没有自己想象的那样。

还得搜索，找到一个国外的博主写的更新方式，比较靠谱，操作如下：

1. 找到你的hexo的目录，就是你使用hexo init 初始化的那个目录

2. 找到里面的 **package.json** 文件

3. 使用vscode打开，然后找到里面 "dependencies" 后面大括号里面的 `"hexo": "^3.9.0"` 把里面的版本号3.9.0更改为现在的新的版本号，我更新的是 4.2.1

4. 文件里面的这个不用更改

   ```json
    "hexo": {
       "version": "4.2.1"    #这个不用更改
     },
   ```

   我刚开始就是改的这里，然后没有效果，我还以为是博主写错了呢。

5. 更改保存后执行 `npm update`

## hexo d部署出错

生活就是这么艰难，部署又出错了，我都无语了。出错如下：

```bash
[proxychains] config file found: /usr/local/etc/proxychains.conf
[proxychains] preloading /usr/local/Cellar/proxychains-ng/4.14/lib/libproxychains4.dylib
```

网上搜索以后发现问题还是版本的问题，这次是因为node版本太高，f**k。只能再安装一个低版本，一般选择最新的稳定版就可以。

## 安装多版本的node

方法很多，我这里用的node的版本管理模块n，操作如下

```bash
# 安装管理模块
sudo npm sudo npm install n -g
# 安装稳定版
sudo n stable
# 安装最新版
sudo n latest
# 查看所安装版本
n
# 选择使用的版本
sudo n 版本号
# 帮助
n -h
```

参考博文：

- [How to Update Hexo?](https://finisky.github.io/2019/11/24/updatehexo/) 

- [部署Hexo踩过的坑—node14.0配置hexo](https://zhuanlan.zhihu.com/p/136552969)

- [安装更高版本的node后怎么安装低版本的？](https://m.html.cn/qa/node-js/12280.html)
