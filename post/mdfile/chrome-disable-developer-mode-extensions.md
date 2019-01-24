---
title: Chrome禁止“请停用以开发者模式运行的扩展程序”的提示
date: 2018-12-22
categories: Web
tags:
- Windows
- chrome developer
- disable
---

在Windows7系统下的Chrome里面使用开发者模式导入了一个插件，就是为了登录一下最大的那个搜索引擎（[传送门](https://github.com/haotian-wang/google-access-helper)）。然后每次登录就会出现“请停用以开发者模式运行的扩展程序”的提示，而且必须点击一下才能关掉。

去网络搜索了一下，操作还挺复杂，具体的操作步骤等会我写在下面，不过如果懒得自己操作，国外有高手写了批处理文件，我也会放在下面。

<!-- more -->

# 第一种方法

如果你经常使用自己导入的插件，建议使用`Chrome Canary`版本，这个是没有提示的，而且图标是金色的哦。下载地址可以看一下[这里](https://aoenian.github.io/2016/08/08/chrome-plugin/)

# 第二种方法

如果你想使用稳定版，但是又不想看到那个提示，请按照下面的操作处理。

如果你不想自己操作，请下载批处理文件[DevWarningPatch.bat](https://raw.githubusercontent.com/aoenian/myconfigfiles/master/windows/DevWarningPatch.bat) **请使用管理员模式运行**。如果觉得不安全，或者文件失效请移步文末的参考文章，那里是代码的原地址。

手动操作步骤如下：

转自：[stackoverflow](https://stackoverflow.com/a/30361260) 建议大家还是查看原文。下面是引用内容

>you can patch it manually using [HIEW](http://www.hiew.ru/):
>
>1. exit Chrome fully by pressing `Ctrl+Shift+Q` and terminating any chrome.exe processes in Windows Task Manager's `Processes` tab (it's a second tab, not the default tab).
>
>2. open chrome.dll in hiew
>
>3. press `Enter` to switch the view
>
>4. press `F7`, paste `ExtensionDeveloperModeWarning` in the first input field, press `Enter`
>
>5. press `F3` to enter the edit mode
>
>6. type `00`
>
>7. press `F9` to save
>
>8. press `Left` to position the cursor on 00
>
>9. press `F6` to find the reference
>
>![L4yJa.png](https://i.stack.imgur.com/L4yJa.png)
>
>10. look for `cmp eax,3` or `cmp eax,2` on the screen and the adjacent few pages up and down
>
>![o0ozy.png](https://i.stack.imgur.com/o0ozy.png)
>
>11. navigate the cursor using `Up` and `Down` to that line's binary code (a column on the left)
>
>12. press `F3` to enter the edit mode
>
>13. navigate the cursor using `Left` and `Right` to `03` or `02` in that line's code
>
>14. type `09`
>
>15. press `F9` to save
>
>16. Close hiew, otherwise Chrome won't launch.

参考文章：

- [How to get rid of “disable developer mode extensions” pop-up](https://stackoverflow.com/questions/30287907/how-to-get-rid-of-disable-developer-mode-extensions-pop-up#)
