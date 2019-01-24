---
title: 鼠须管更新0.10.0部署出错解决
date: 2019-01-24
categories: macOS
tags:
- Rime
- error
---

今天刚打开电脑然后Rime输入法更新提示弹出来了，真的很惊喜，一直以为这个输入法不会更新了。_不过前几个月Windows上的已经更新了，想来macOS上更新也就不奇怪了。Linux上面可能就没这么好运了。_

点击更新以后，保险期间重启了电脑，然后重新部署，谁知道居然出错了，但是却不影响输入，对了可能是因为我用的**小鹤双拼**。

首先找到出错信息的位置，提示出错信息在`$TMPDIR`中。在终端中输入`env`命令查看`$TMPDIR`的具体位置。通过终端进入相应的目录后，可以执行`open .`命令在当前目录打开**Finder**查看。

<!--more-->

查看`rime.squirrel.ERROR`里面的出错信息发现如下提示：

```shell
# 意思就是提示好多输入方案需要的文件不存在
"stroke.reverse.bin
luna_pinyin_simp.prism.bin
luna_pinyin_tw.prism.bin
...
"
No such file or di rectory
```

解决方法在[这里](https://github.com/rime/squirrel/issues/279)主要是因为新版精简了安装包，如果需要预设输入方案或其他输入方案请执行以下命令

```shell
# 安装预设输入方案
curl -fsSL https://git.io/rime-install | bash -s -- :preset
# 安装双拼方案
curl -fsSL https://git.io/rime-install | bash -s -- :preset double-pinyin  
```

其他的输入方案可参考[plum](https://github.com/rime/plum)。
