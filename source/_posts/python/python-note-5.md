---
title: python学习笔记（五）---基础IO操作
categories:
  - python
tags:
  - python
date: 2018-08-02 20:56:42
---



[*依据廖雪峰官方网站的python教程整理* ](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-


__author__ = "KingRumn"

'''
    基础IO操作
    文件读写、StringIO、BytesIO、文件和目录操作、序列化
'''

from io import StringIO
from io import BytesIO
import os
import pickle

# 文件IO
# 读文件
# 第一步： 打开文件
'''
    open(name[, mode[, buffering]])
    name : 一个包含了你要访问的文件名称的字符串值,接受绝对路径和相对路径;
    mode : mode 决定了打开文件的模式：只读，写入，追加等,默认为只读(r);
    buffering : 如果 buffering 的值被设为 0，就不会有寄存。如果 buffering 的值取 1，访问文件时会寄存行。
                如果将 buffering 的值设为大于 1 的整数，表明了这就是的寄存区的缓冲大小。
                如果取负值，寄存区的缓冲大小则为系统默认
    r：只读，指针位于开头，不存在则报错；rb：二进制只读；r+：可读可写，打开文件，指针位于文件头；rb+：二进制可读写；
    w：写入，如有文件，删除内容，否则创建文件；wb：二进制可写；w+:读写；wb+：二进制读写；
    a:追加，指针位于文件末尾，文件不存在，创建文件；ab：二进制追加；a+：追加读写；ab+：二进制追加读写；
    
'''

# f=open('notfound.txt', 'r')  # FileNotFoundError: [Errno 2] No such file or directory: '/Users/michael/notfound.txt'
f = open('test.txt', 'r')

# 第二步：读取文件
# read()方法可以一次读取文件的全部内容，用一个str表示
print(f.read())     # 'test line 1\ntest line 2'

# 第三步：关闭文件
f.close()

# 利用try...finally保证close的正确调用
try:
    f = open('test.txt', 'r')
    print(f.read())
finally:
    if f:
        f.close()

# 利用with简化close调用
with open('test.txt', 'r') as f:
    print(f.read())

# 调用readline()可以每次读取一行内容
with open('test.txt', 'r') as f:
    print(f.readline())        # test line 1

# 调用readlines()一次读取所有内容并按行返回list
with open('test.txt', 'r') as f:
    for line in f.readlines():
        # 把末尾的'\n'删掉
        print(line.strip())  # test line 1

# file-like object
# 不要求从特定类继承，只要写个read()方法就行

# 利用encoding函数指定编码格式
# errors='ignore' 可以避免一些不规范的编码字符抛出异常
with open('test.txt', 'r', encoding='utf-8', errors='ignore') as f:
    print(f.readline())

# 写文件
with open('test.txt', 'w') as f:
    f.write('Hello, world!')

# StringIO：内存中读写str
# from io import StringIO
# 创建IO
f = StringIO()

# 写入内容
f.write('hello\nTom\ncatch out')

# 读取内容
print(f.readline())
print(f.getvalue())

# 利用str直接初始化StringIO
f = StringIO('Hello!\nHi!\nGoodbye!')
print(f.getvalue())

# BytesIO: 在内存中读写bytes
# from io import BytesIO
f = BytesIO()
f.write('中文'.encode('utf-8'))
print(f.getvalue())

# 用二进制数据直接初始化BytesIO
f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87')
print(f.getvalue())

# 操作文件和目录
# import os

# 获取操作系统名称
# 如果是posix，说明系统是Linux、Unix或Mac OS X，如果是nt，就是Windows系统
print(os.name)

# 获取环境变量
print(os.environ)       # 'C:\\Windows\\system32;...'
# 获取某个环境变量
print(os.environ.get('path'))   # 'C:\\Users\\xxx\\AppData\\Roaming'

# 获取当前绝对路径
print(os.path.abspath('.'))
# 连接路径： os.path.join()函数，这样可以正确处理不同操作系统的路径分隔符
print(os.path.join('/Users/michael', 'testdir'))
# 拆分路径
print(os.path.split('/Users/michael/testdir/file.txt'))
# 创建目录
os.mkdir('testdir')
# 删除目录
os.rmdir('testdir')
# os.path.splitext()可以直接让你得到文件扩展名
os.path.splitext('/path/to/file.txt')   # ('/path/to/file', '.txt')
# 重命名
os.rename('test.txt', 'test.py')
os.rename('test.py', 'test.txt')
# 列出当前路径下的所有目录
print(list([x for x in os.listdir('.') if os.path.isdir(x)]))
# 列出当前路径下的py文件
print(list([x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1] == '.py']))

# 序列化
# 把变量从内存中变成可存储或传输的过程称之为序列化
# 序列化之后，就可以把序列化后的内容写入磁盘，或者通过网络传输到别的机器上
# 反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling
# pickle模块实现序列化
# import pickle
d = dict(name='Bob', age=20, score=88)

# 序列化
with open('dump.txt', 'wb') as f:
    pickle.dump(d, f)

# 加载
with open('dump.txt', 'rb') as f:
    dl = pickle.load(f)
    print(dl)
    print(d)
```





 更多更及时的博客更新请戳--->  [KingRumn](http://zm913.xyz "KingRumn的Blog")

