---
title: Python装饰器（一）
date: 2019-07-24
categories: Python
tags:
- 装饰器
---

这段时间学习Python，之前已经学习过基本的语法，现在准备重新拾起来，看的教程是github上面的[Python - 100天从新手到大师](https://github.com/jackfrued/Python-100-Days)。进度相对一般的教程比较快，虽然没有那么详细，但是能够快速了解整体的框架。

第九天的教程里面讲到了装饰器，这个之前了解过，但是一直似懂非懂，加上之前用的比较少，也是看过就算了。这次又出现了，不能再糊弄过去，要认真理解透彻。这里把个人的理解过程记录下来。

<!-- more -->

## 参考博客

这里先放出来我查看过的资料,如果你看过以后懂了，那么恭喜，下面我写的内容就不用看了。

- [Python装饰器为什么难理解？](https://foofish.net/understand-decorator.html)

- [理解 Python 装饰器看这一篇就够了](https://foofish.net/python-decorator.html)

- [装饰器](https://www.liaoxuefeng.com/wiki/1016959663602400/1017451662295584)

## 简单的函数装饰器

装饰器在Python中很强大，扩展函数功能而不改变函数的代码，同时能够多次复用。在查看完上面的资料后我个人还有几点没有弄清楚，后来慢慢有了自己的一些理解，记录如下。

### 函数是变量

一定要先理解在Python语言中，一切皆对象，函数也能当变量。那么赋值，作为另一个函数参数这些都不在话下。

```py
def foo():  # 最简单的函数
    print('i am foo')

bar = foo   # 这里注意赋值的时候是函数名字，不能加括号
bar()       # 这时调用bar(),结果与 foo()相同,类似又给foo函数起了一个别称
bar.__name__    # 输出 foo
```

看完赋值，接着再看一个例子，函数作为另一个函数的参数

```py
def foo():
    print('i am foo')

# bar函数接收的参数是另一个函数名 当然func只是一个名字可以用其他名字替换
def bar(func):  
    print('i am bar')
    func()      # 这里调用了传入的函数

bar(foo)        # 先打印出 i am bar, 然后调用 foo 再打印出 i am foo
```

上面的函数都很简单，没有返回值，而且函数内部也没有再次定义函数，再看一个函数内部定义函数，然后并返回的例子。

```py
def bar():
    def foo():      # 定义函数 foo
        print('i am foo')
    return foo      # 返回函数名，注意是函数名并没有括号

b = bar()       # 用b接收bar的返回值
print(b.__name__)       # 输出 foo
b()         # 输出 i am foo
```

上面的例子就是把函数作为返回值返回，这时候返回的函数就会赋值给b，当执行`b()`的时候即执行了`foo()`。

## 装饰器

看到这里就可以谈谈装饰器了，装饰器的作用个人感觉就是在要执行的函数外面再加上一层函数来实现一些非核心的功能，这样能够避免破坏原函数的结构，同时还能实现增加功能的需求。

以上面博客中常用的打印日志的代码为例：

```py
def log(func):
    """打印日志"""
    print('%s start' % func.__name__)
    func()
    print('%s end' % func.__name__)

def foo():
    print('i am foo')

log(foo)    # 这样就可以在foo函数执行前后完成日志的输出
```

这样做是可以的，不过缺点上面的博客也提到了，这样做就等于调用了`log`函数，跟原来的函数`foo`没啥关系了。同时还有一点注意就是这里直接执行`log(foo)`即可得到结果，`log`函数**没有返回值**，所以也没有赋值给其他的变量。

### 装饰器的效果

装饰器的语法就不多说了，而装饰器的效果是装饰器接收真正的业务函数然后把返回的内容再次赋值给业务函数名，以此来增强业务函数的功能。了解了这个闪腰的操作你就不会纠结为啥装饰器要那么费劲再返回函数。上例子

```py
def log1(func):
    """打印日志，没有任何返回"""
    print('%s start' % func.__name__)
    func()
    print('%s end' % func.__name__)

def log2(func):
    """打印日志，返回函数"""
    def wrapper():
        print('%s start' % func.__name__)
        func()
        print('%s end' % func.__name__)
    return wrapper

@log1           # foo = log1(foo)  直接输出日志语句
def foo():      # 不用调用foo()即可直接输出
    print('i am foo')       # 显然这个结果不是我们想要的

@log2           # foo = log2(foo) = wrapper
def foo():
    print('i am foo')

foo()           # 调用foo() 即执行 wrapper() 这时才会输出日志
```

### wrapper函数为什么返回func()

其实到这里基本概念就差不多了，但是我在查看教程的时候发现wrapper函数都是直接返回func()，这里一直不明白，直接调用就好了啊，为什么要返回呢。后面我测试了一些例子，发现高手还是高手。

```py
def log(func):
    """打印日志，返回函数"""
    def wrapper():
        print('%s start' % func.__name__)
        # func()
        return func()
    return wrapper

@log
def foo():
    print('i am foo')   # 当没有返回值的时候 wrapper函数是否返回基本区别不大

@log
def foo2():
    return('i am foo')  # 当有返回值的时候 wrapper函数是否返回就有区别了

# 如果不用 return func() 则无法得到foo2的返回值，也就无法输出  i am foo
print(foo2())   # 如果wrapper中不使用 return func() 则输出 None
```

当然如果再想的多点，为什么wrapper返回的是`func()`，不是`func`。而log返回的则是函数名`wrapper`。

### 如果没有wrapper函数会怎样

这个问题我也没想到，在知乎的问答中看到的地址如下：[点我](https://www.zhihu.com/question/26930016/answer/99243411)，我把问题复现如下：

```py
def log(func):
    print("this is log")
    func()
    return func

@log
def foo():
    print("this is foo function")

# 请执行两次，你会惊奇的发现两次执行的结果不一样
foo()
print('***')
foo()

'''
输出内容如下：
this is log
this is foo function
this is foo function
***
this is foo function
'''

```

**分析一下：**其实如果你此时正在郁闷，说明你也被欺骗了，请在第一个`foo()`前增加一行`print('***')`，真相就会出现。因为在装饰器运行的时候执行如下命令`foo=log(foo)`这个时候就会输出前2行内容。然后`log(foo)`返回的`func`赋值给了`foo`，所以`foo`根本没变，执行后就输出一行内容**this is foo function**。

其实上面的这个例子也解答了，为什么那些教程中的例子把函数的代码内容都放在`wrapper`函数中，而没有放在最外层的`log`中。

## 带参数的装饰器

到这里，基本函数装饰器的最简单的使用应该理解差不多了，后面就是在以上基础上再增加功能，如被装饰的函数有参数怎么办，装饰器需要参数怎么办，多个装饰器如何工作。

```py
def log(level):
    def decorator(func):
        def warpper(*args, **kw):
            if level > 3:
                print('High')
            else:
                print('Low')
            return func(*args, **kw)
        return warpper
    return decorator

@log(5)
def foo(name):
    print('%s is foolish' % name)

foo('aoenian')
```

这里的装饰器的作用是`foo = log(5)(foo)`，则`foo = decorator(foo) = warpper`，这时再调用`foo('aoenian')`实则为`warpper('aoenian')`。

这次先分析这么多，还有一些用法后续再补充，如果有更好的理解方法或思路，欢迎留言探讨。
