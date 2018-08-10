---
title: python获取当前系统类型：os.name, sys.platform, platform.system对比 
categories:
  - python
tags:
  - python
date: 2018-08-09 10:35:02
---



在很多情况下，需要在python中获取当前系统的类型，用于判断是unix/windows/mac或者java虚拟机等，python中提供了os.name， sys.platform， platform.system等方式，但是起初并不知道各个函数之间的区别以及在什么情况下用那种。

# 函数简介

首先，分别对每个函数进行说明，明确其提供的能力，以及如何使用。

## os.name

官方文档（v3.6.6）解释如下：

> os.name
>
> 
>
> The name of the operating system dependent module imported. The following names have currently been registered: `'posix'`, `'nt'`, `'java'`

该变量返回当前操作系统的类型，当前只注册了3个值：分别是`posix` , `nt` , `java` 

使用方法：

```python
import os
print(os.name)
```



## sys.platform

> sys.platform
>
> 
>
> This string contains a platform identifier that can be used to append platform-specific components to [`sys.path`](https://docs.python.org/3.6/library/sys.html?highlight=sys#sys.path), for instance.

该变量返回当前系统的平台标识。

对除了linux的Unix系统， 是`uname -s` 的返回值和`uname -r` 版本合并的小写形式，例如linux2指的是使用2.x.x版本的linux系统。由于包含了系统版本，故推荐的使用方法是借助`startwith` 函数。

```python
import sys
if sys.platform.startwith('linux')
    # linux 代码
    pass
 elif sys.platform.startwith('freebsd')
    # freebsd 代码
    pass
```

对windows系统， 返回值参考下表：

| 系统             | 返回值      |
| -------------- | -------- |
| window         | ’win32‘  |
| linux          | ’linux‘  |
| Windows/Cygwin | 'cygwin' |
| Mac OS X       | 'darwin' |

这些值在编译时已经确定了。

## platform.system()

> platform.system()
>
> 
>
> Returns the system/OS name, e.g. `'Linux'`, `'Windows'`, or `'Java'`. An empty string is returned if the value cannot be determined.

返回当前操作系统的名字，例如`Linux` , `Windows` , `Java` , ...如果不能确定会返回空字符串

这是在上层的脚本实现，需要在运行时执行。



# 主要区别

* `sys.platform` 在构建配置时指定的编译器定义
* `os.name` 用于检查特定的某些模块是否可用（例如：POSIX，nt， ...）
* `platform.system()` 实际上运行uname和潜在的几个其他函数来确定运行时的系统类型





 更多更及时的博客更新请戳--->  [KingRumn](http://zm913.xyz "KingRumn的Blog")

