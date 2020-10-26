---
title: Windows下命令行软件包管理工具Scoop避坑备忘录
date: 2019-12-16 16:58:24
categories: Windows
tags:
- soft
- manage
---

在配置计算机开发环境的时候，相应的软件管理其实一直都是一个很重要的需求。macOS上面有[Homebrew](https://brew.sh/)，Linux方面就不用多说了apt、dnf等等，而Windows上面则一直都没有能够拿得出手的管理方法。不过今天介绍的这个工具的名字为[Scoop](https://github.com/lukesampson/scoop)官方的介绍是**A command-line installer for Windows**，这个可能正是你想要的。

<!--more-->

## 一些介绍

**痛苦的回忆：**依稀还记得学习java的时候，找到官方网站-下载jdk-安装jdk-配置环境变量一直是一个噩梦，多少渴望知识的幼小心灵直接在此受到暴击一蹶不振。虽然现在越来越方便了，但是整个配置流程也几乎没有太大的改观。在国内的网络环境如果想安全的配置好环境，依然需要不断的重复官网下载，安装配置的过程。（当然，官网打不开，下载速度慢等等就不多说了，都是泪）

回到正题，今天介绍的Scoop正是为了让你解脱而出现的，项目建立的比较早了，但是自己到现在才知道（朋友介绍的，嘿嘿），惭愧惭愧。废话有点多，这个Scoop主要的特点（摘录自官网，反正就是很给力，主要我也看不懂，还有一个类似功能的叫做[Chocolatey](https://chocolatey.org/docs)）：

- Permission popup windows
- GUI wizard-style installers
- Path pollution from installing lots of programs
- Unexpected side-effects from installing and uninstalling programs
- The need to find and install dependencies
- The need to perform extra setup steps to get a working program

## 安装前准备

摘录自[官网文档](https://github.com/lukesampson/scoop/wiki/Quick-Start)

- Windows 7 SP1+ / Windows Server 2008+
- PowerShell 5 (or later, include PowerShell Core) and .NET Framework 4.5 (or later)
- PowerShell must be enabled for your user account e.g. Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

我这里给个个人建议：**安装Windows10并更新到最新的补丁即可。** 如果你想安装新版本的.net，可以参考[windows10 如何安装 .net](https://docs.microsoft.com/zh-cn/dotnet/framework/install/on-windows-10)。

如果你不想换系统，那么需要查看你的powershell和.net是否符合。方法如下：

- 启动powershell，执行命令`$PSVersionTable`返回的内容中 psversion 为powershell版本
clrversion 为.net版本 （这个我感觉不是太清楚）。还可以使用命令`$psversiontable.psversion.major`来查看，返回值要>=5.0

- 查看系统安装的所有的.net版本，执行命令

    ```sh
    # 查看所有版本
    dir 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' | sort-object name -Descending | select-object -ExpandProperty PSChildName

    # 另一种方法
    (Get-ItemProperty "HKLM:SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full").Release
    ```

.net的版本信息有点乱，具体可以参照官网的解释，摘录如下：

> 每个新版本的 .NET Framework 都会保留早期版本中的功能并会添加新功能。 可在同一台计算机上同时加载多个版本的 .NET Framework，这意味着可安装 .NET Framework 而无需卸载以前的版本。 通常，你不应卸载以前版本的 .NET Framework，因为你使用的应用程序可能依赖于特定版本，如果删除该版本，可能会中断。
> .NET Framework 版本和 CLR 版本之间存在差异：
> .NET Framework 版本基于构成 .Net Framework 类库的一组程序集。 例如，.NET Framework 版本包括 4.5、4.6.1 和 4.7.2。
> CLR 版本基于 .NET Framework 应用程序执行的运行时。 单个 CLR 版本通常可支持多个 .NET Framework 版本。 例如，CLR 版本 4.0.30319.xxxxx 支持 .NET Framework 版本 4 到 4.5.2（其中 xxxxx 小于 42000），而 CLR 版本 4.0.30319.42000 支持从 .NET Framework 4.6 开始的 .NET Framework 版本。

## 正式安装

最权威的文档是官方的[quick start](https://github.com/lukesampson/scoop/wiki/Quick-Start)

执行如下命令：

```sh
# 这条命令一定要执行，会返回很多选项，请选择 A (全是)，这是坑1，不然下面就会不断出错
Set-ExecutionPolicy RemoteSigned -scope CurrentUser

# 安装命令，如果上面选择全是，还有你的网络不是太差的话应该就没问题了
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```

如果上面的安装命令还是一直出错，出错提示为**使用“1”个参数调用“DownloadString”时发生异常:“无法连接到远程服务器”**，那么恭喜你，你的网络真的够垃圾。可以试试下面的这个处理方法，因为地址<https://get.scoop.sh>的真实地址是<https://raw.githubusercontent.com/lukesampson/scoop/master/bin/install.ps1>，这里用真实的地址替换原来的地址，即安装命令修改如下：

```sh
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/lukesampson/scoop/master/bin/install.ps1')
```

## 安装应用软件

上面完成后你就可以安装一些很常用的软件了，比如7z git aria2 vim等，使用方法：

```sh
# 安装
scoop install aria2 7zip git python
# 卸载
scoop uninstall aria2
# 列出安装软件以及状态
scoop list
# 查找软件
scoop search python
# 帮助
scoop help <command>
```

这里的坑是：1.**建议你挂个梯子，不然下载软件的过程会让你生不如死**, 2.**软件安装失败再次安装的时候需要先卸载再重装** 3. **7z如果无法识别压缩包，那么找到7z的启动程序右键管理员启动，然后关联所有的压缩文件即可。**

这里面遇到一个问题比较特殊，我安装了aria2以后scoop自动调用，但是aria2下载速度特慢，然后我根据提示禁用了aria2但是依然调用，后面把aria2卸载以后速度快多了。

## 增加软件仓库

官方的很少，而且连chrome、firefox都没有，所以需要增加一些额外的仓库来满足我们的需要，这里注意**安装额外仓库需要先安装git**。

```sh
# 添加extras bucket，其他仓库直接更换 extras 为仓库名即可
scoop bucket add extras
```

## 个人体会

最后写一些自己使用过程中的体会（windows10 64位）

- 安装过程全自动
- 软件安装位置默认为你的用户目录下面的scoop目录
- 由于众所周知的原因，下载软件是真的慢，而且几乎不可能一次成功
- 还是建议使用代理进行操作不然体验还不如去官网下载安装
- 软件安装完成后如果有图形界面，则会在启动程序-scoop目录下面添加快捷方式（也可能没有）

总结一下就是，**建议使用，管理方便，配置不乱**。

参考资料：

- [Scoop](https://github.com/lukesampson/scoop)
- [extras里面的软件包](https://github.com/lukesampson/scoop-extras/tree/master/bucket)
- [buckets](https://github.com/lukesampson/scoop/blob/master/buckets.json)
