---
title: linux命令中的一些冷操作1
date: 2020-05-03 21:01:32
categories: Linux
tags:
- date
- paste
- find
- xargs
---

在学习linux的基本命令的过程中遇到了一些问题，有些是按照教程执行没有效果，有些是心血来潮猜测是否能够有其他的操作，这里把这些结果记录下来，避免遗忘后重复搜索，浪费时间。

## 合并多个文本

这个比较简单的，使用`cat`命令即可完成，不过之前没有想到，这里还能够使用通配符，能够一次合并多个文件。

举个例子： `cat file*.txt > fileall.txt` 当然要先根据内容把文件重命名，做好排序。

windows下分两种情况（系统是windows10）：
<!--more-->
- cmd中命令使用都没问题

    ```bash
    # 使用copy命令
    copy *.txt all.txt
    copy 1.txt + 2.txt + 3.txt all.txt
    # 使用type命令
    type *.txt > all.txt
    ```

- powershell中则比较复杂

    ```bash
    # copy命令的问题
    copy *.txt all.txt    # 只能复制最后文件的内容
    copy 1.txt + 2.txt + 3.txt all.txt    # 路径问题命令出错
    # type命令
    # 这个命令会无限循环把all.txt中内容再次加入all.txt中
    type *.txt > all.txt
    # 可以采用不同的后缀避免
    type *.txt > all    # 这样就可以了
    ```

## date命令无法修改日期

在练习date命令的时候，发现无法修改系统的日期（root也不行），但是教程上是可以的。搜索后发现应该是开启ntp的原因。可以通过下面的命令来处理。

```bash
# 关闭ntp，0替换为false也可
sudo timedatectl set-ntp 0
# 直接用timedatectl命令修改或用date修改都可
timedatectl set-time "2020-05-01 18:18:08"
timedatectl set-timezone Erope/Paris
timedatectl set-local-rtc 0
```

## 软链接

学习软链接命令的时候突然想起来的，看代码吧，不好说清楚

```bash
$ touch file
$ ln -s file filesoft
$ echo "hello" > filesoft   # 链接没问题，内容直接写入file
$ cat file
hello
$ mv file file1
$ echo "world" > filesoft   # 链接失效，系统自动创建file并写入
$ ls
file  file1  filesoft
$ cat file file1
world
hello
```

**结论：**软链接失效后，如果链接的是文件则系统会自动重建，如果是目录则提示No such file or directory

## dd命令硬盘测试

网络上很多，这里记录一下

```bash
# 测试写入速度
time dd if=/dev/zero of=/tmp/test1.img bs=128M count=8 oflag=dsync
# 读取速度
time dd if=/path/to/bigfile of=/dev/null bs=8k
```

这里注意dev目录下的四个特殊设备，内容摘自 [Linux中的虚拟设备](https://blog.csdn.net/sinat_26058371/article/details/86754683)

```bash
# /dev/null 空设备，一般称为黑洞，输入这里的数据直接丢弃
find / -name "hello" 2> /dev/null
#将标准输出和错误输出重定向到/dev/null，运行这个脚本不会输出任何信息到终端
$ ./run.sh 1>/dev/null 2>&1  
# /dev/zero 零设备，可以无限的提供空字符（0x00，ASCII代码NUL）
# 能够生成指定大小文件
dd if=/dev/zero of=/tmp/100Mfile bs=1M count=100
# /dev/urandom /dev/random
# 随机数设备，提供不间断的随机字节流
# /dev/random产生随机数据依赖系统中断,产生数据速度较慢，但随机性好
# /dev/urandom不依赖系统中断，数据产生速度快，但随机性较低
cat /dev/random | od -x
cat /dev/urandom | od -x | head -n 5
# 利用/dev/urandom设备产生一个128位的随机字符串
str=$(cat /dev/urandom | od -x | tr -d ' ' | head -n 1)
echo ${str:7}
17539187d2e8b8e26d49bec90465c14d
```

## paste命令如何使用多个分隔符

paste命令在合并文件的时候使用多个分隔符，这个应该是无法直接实现，利用其他方法

```bash
# 使用sed命令替换
paste -d '~' file1 file2 | sed 's/~/,,/'
# 使用 /dev/null 作为承接 脑洞很大
paste -d, file1 /dev/null file2
```

## find的exec和xargs

find很强大，里面的exec选项能够让find更加如虎添翼，-exec 参数后跟command，为了保证shell解释正确使用 `\;` 结束命令。同时`{}`代表find查出来的结果，有时避免shell过滤也使用`'{}'`。

xargs则常与find结合使用来避免-exec选项出现的参数溢出的问题。xargs能够捕获一个命令的输出，然后传递给另外一个命令，由于很多命令不支持使用管道来传递参数，即不支持从标准输出中读取数据，所以xargs很常用。

```bash
find / -name 'bash*' -exec mv {} /tmp \;
find / -empty | xargs rm -rf
# 实现比较
# print选项把查找结果的文件名显示出来
find ./ -type f -name "*.sh" -exec grep "root" {} \; -print
# xargs显示的信息比较全
find ./ -type f -name "*.sh" | xargs grep "root"
# exec选项的安全模式，把exec替换为ok
find ./ -name "*.log" -ok rm {} \;
```

参考资料：

- [xargs rm -rf 与 -exec rm](https://www.cnblogs.com/fjping0606/p/5670661.html)
- [find命令之exec](https://www.cnblogs.com/peida/archive/2012/11/14/2769248.html)
- [find命令的-exec选项为何不能用管道命令代替?](https://segmentfault.com/q/1010000000719166)
- [Linux xargs 命令](https://www.runoob.com/linux/linux-comm-xargs.html)
- [linux 使用rename命令批量重命名文件](https://blog.csdn.net/fdipzone/article/details/44604591)  
- [linux中合并多个文件内容到一个文件的例子](https://blog.csdn.net/Alvin_Lam/article/details/79123882)
- [Linux and Unix Test Disk I/O Performance With dd Command](https://www.cyberciti.biz/faq/howto-linux-unix-test-disk-performance-with-dd-command/)
