---
title: python学习笔记（七）---多线程
categories:
  - python
tags:
  - python
date: 2018-08-02 21:40:55
---


[*依据廖雪峰官方网站的python教程整理* ](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)


```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-


__author__ = "KingRumn"

'''
    多线程编程，
'''

import time
import threading

# Python的标准库提供了两个模块：_thread和threading，
# _thread是低级模块，threading是高级模块，对_thread进行了封装。
# 绝大多数情况下，我们只需要使用threading这个高级模块
# 启动一个线程就是把一个函数传入并创建Thread实例，然后调用start()开始执行


# 新线程执行的代码:
def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)


print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')

# 调用start()开始执行
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)

# 多线程和多进程最大的不同在于，
# 多进程中，同一个变量，各自有一份拷贝存在于每个进程中，互不影响，
# 而多线程中，所有变量都由所有线程共享，所以，任何一个变量都可以被任何一个线程修改，
# 因此，线程之间共享数据最大的危险在于多个线程同时改一个变量，把内容给改乱了

# 假定这是你的银行存款:
balance = 0


def change_it(n):
    # 先存后取，结果应该为0:
    global balance
    balance = balance + n
    balance = balance - n


def run_thread(n):
    for i in range(100000):
        change_it(n)


t1 = threading.Thread(target=run_thread, args=(5,))
t2 = threading.Thread(target=run_thread, args=(8,))
t1.start()
t2.start()
t1.join()
t2.join()
print(balance)

# t1和t2是交替运行导致balance的结果有可能不是0
# 通过加锁，控制全局变量的访问
balance = 0
lock = threading.Lock()


def change_it_a(n):
    # 先存后取，结果应该为0:
    global balance
    balance = balance + n
    balance = balance - n


def run_thread_a(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it_a(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()


t1 = threading.Thread(target=run_thread_a, args=(5,))
t2 = threading.Thread(target=run_thread_a, args=(8,))
t1.start()
t2.start()
t1.join()
t2.join()
print(balance)


# Python的线程虽然是真正的线程，但解释器执行代码时，有一个GIL锁：Global Interpreter Lock，
# 任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。
# 这个GIL全局锁实际上把所有线程的执行代码都给上了锁，
# 所以，多线程在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。
# Python虽然不能利用多线程实现多核任务，但可以通过多进程实现多核任务。多个Python进程有各自独立的GIL锁，互不影响

```



 更多更及时的博客更新请戳--->  [KingRumn](http://zm913.xyz "KingRumn的Blog")

