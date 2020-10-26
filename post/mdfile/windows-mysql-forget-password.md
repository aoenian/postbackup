---
title: Windows系统下MySQL忘记密码后如何重置
date: 2019-07-15
categories: Windows
tags:
- password
- mysql
---

这段时间试着用了一下MySQL，谁知道隔得时间长了忘记了密码，在网络搜索后发现可以通过命令重置密码。原文的部分操作并不是太详细，这里把自己的具体操作介绍如下，希望能帮到大家。

<!--more-->

## 系统环境

环境是Windows7，MySQL的版本是5.5.

## 操作步骤

1. 关闭正在运行的MySQL。

    具体操作是 ：ctrl+alt+delete—>任务管理器—>结束mysql.exe。

2. cmd命令打开DOS窗口，转到mysql\bin目录。

    具体操作是 ：cd C:\Program Files\MySQL\MySQL Server 5.5\bin(默认安装目录，自定义目录类同)。

3. 输入`mysqld -nt --skip-grant-tables`（注意mysqld与-nt之间有一空格）

4. 再开一个DOS窗口（因为刚才那个DOS窗口已经不能动了），转到mysql\bin目录（操作同步骤2）。

5. 输入mysql回车，如果成功，将出现MySQL提示符 > 。如果不成功，可能是mysql.exe没关，关闭MySQL以后，再次重复第5步骤

6. 连接权限数据库>`use mysql;` (>是本来就有的提示符,别忘了最后的分号)

7. 改密码：> `update user set password=password("123456") where user="root";` (别忘了最后的分号，这里把密码改为123456)

    刷新权限（必须的步骤）>`flush privileges;`

8. 退出 > `\q`

参考博文：

- [MySQL账号密码忘记解决方法](https://blog.csdn.net/cn_lyg/article/details/74157432)

- [Mysql5.7.18.1修改用户密码报错ERROR 1054 (42S22): Unknown Column 'Password' In 'Field List'解决办法](https://www.cnblogs.com/wangbaobao/p/7087032.html)
