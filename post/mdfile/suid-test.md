---
title: 文件特殊权限SUID测试
date: 2017-11-02
categories: Linux
tags:
- SUID
---


测试方法1（失败）
=================

思路:使用shell脚本，然后把脚本编译为二进制，赋予 suid
权限，用不同的用户执行这样就能比较是否执行的时候用户是文件的所属用户

安装shc编译shell脚本
--------------------

``` bash
[root@centos-rpi3 shc]# yum install automake
[root@centos-rpi3 tmp]# git clone https://github.com/neurobin/shc.git
[root@centos-rpi3 shc]# cd shc
[root@centos-rpi3 shc]# mkdir m4
[root@centos-rpi3 shc]# ./autogen.sh
[root@centos-rpi3 shc]# ./configure
[root@centos-rpi3 shc]# make
[root@centos-rpi3 shc]# make install
```

<!--more-->

测试环境配置
------------

``` bash
[aoenian@centos-rpi3 tmp]$ vim rmfile.sh
# rmfile.sh 
# #!/bin/bash
# rm -i /home/aoenian/file.txt

[aoenian@centos-rpi3 tmp]$ shc -f rmfile.sh -o rmfile
[aoenian@centos-rpi3 tmp]$ su test
[test@centos-rpi3 tmp]$ ./rmfile
[test@centos-rpi3 tmp]$ exit
[aoenian@centos-rpi3 tmp]$ chmod 4755 rmfile
[aoenian@centos-rpi3 tmp]$ su test
[test@centos-rpi3 tmp]$ ./rmfile

```

结果都是失败，都会显示没有权限，这个和设想的就不一样了，然后查看了命令运行时候进程的用户，发现依然是
test 用户在运行 rm 命令

*个人理解：可能执行rmfile程序的时候是aoenian用户，但是rmfile程序调用rm命令的时候就变回来了*

测试方法2（成功）
=================

思路：不使用系统命令，使用C语言编写二进制文件

rmfile.c文件的内容如下：

*会出现警告，不影响使用*

``` c
#include<stdio.h>
int main(){
    char filename[80];
    printf("The file to delete: ");
    gets(filename);
    if( remove(filename) == 0 )
        printf("Removed %s.", filename);
    else
        perror("remove");
    return 0;
}

```

``` bash
[aoenian@centos-rpi3 tmp]$ gcc -Wall rmfile.c -o rmfile
[aoenian@centos-rpi3 tmp]$ touch test.txt
[aoenian@centos-rpi3 tmp]$ chmod 4755 rmfile
[aoenian@centos-rpi3 tmp]$ su test
[test@centos-rpi3 tmp]$ ./rmfile
The file to delete: test.txt
Removed test.txt.

```

测试成功，还是挺有意思的。
