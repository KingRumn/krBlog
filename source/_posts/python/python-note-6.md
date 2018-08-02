---
title: python学习笔记（六）---多进程
categories:
  - python
tags:
  - python
date: 2018-08-02 21:01:55
---


[*依据廖雪峰官方网站的python教程整理* ](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)

# fork进程

```python
import os

# 多进程
# Unix/Linux操作系统提供了一个fork()系统调用，os.fork()封装了fork系统调用，windows系统无法使用
'''
print('Process (%s) start...' % os.getpid())
pid = os.fork()
if pid == 0:
    print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
else:
    print('I (%s) just created a child process (%s).' % (os.getpid(), pid))
'''


```

# 跨平台multiprocessing

```python
from multiprocessing  import Process
import os

# multiprocessing模块就是跨平台版本的多进程模块
# multiprocessing模块提供了一个Process类来代表一个进程对象
# 子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))


if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process will start.')
    # start()方法启动子进程
    p.start()
    # join()方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步
    p.join()
    print('Child process end.')
```

# 进程池pool

```python
from multiprocessing import Pool
import os, time, random


def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))


if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocesses done...')
    # join()之前必须先调用close(), 之后就不能继续添加新的Process了
    p.close()
    #  join()方法会等待所有子进程执行完毕
    p.join()
    print('All subprocesses done.')
```

# 子进程subprocess

```python
import subprocess

# subprocess模块可以让我们非常方便地启动一个子进程，然后控制其输入和输出
print('$ ping www.baidu.com')
r = subprocess.call(['ping', 'www.baidu.com', '-n', '1'])
print('Exit code:', r)

# 如果子进程还需要输入，则可以通过communicate()方法输入
print('$ nslookup')
p = subprocess.Popen(['nslookup'], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
output, err = p.communicate(b'set q=mx\npython.org\nexit\n')
print(output.decode('gbk'))
print('Exit code:', p.returncode)
```

# 进程间通信

```python
from multiprocessing import Process, Queue
import os, time, random


# multiprocessing模块包装了底层的机制，提供了Queue、Pipes等多种方式来交换数据
# 以Queue为例，在父进程中创建两个子进程，一个往Queue里写数据，一个从Queue里读数据
# 写数据进程执行的代码:
def write(q):
    print('Process to write: %s' % os.getpid())
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())


# 读数据进程执行的代码:
def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get(True)
        print('Get %s from queue.' % value)


if __name__ == '__main__':
    # 父进程创建Queue，并传给各个子进程：
    tq = Queue()
    pw = Process(target=write, args=(tq,))
    pr = Process(target=read, args=(tq,))
    # 启动子进程pw，写入:
    pw.start()
    # 启动子进程pr，读取:
    pr.start()
    # 等待pw结束:
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止:
    pr.terminate()

# 在Unix/Linux下，multiprocessing模块封装了fork()调用，使我们不需要关注fork()的细节。
# 由于Windows没有fork调用，因此，multiprocessing需要“模拟”出fork的效果，父进程所有Python对象都必须通过pickle序列化再传到子进程去，
# 如果multiprocessing在Windows下调用失败了，要先考虑是不是pickle失败了。

```



 更多更及时的博客更新请戳--->  [KingRumn](http://zm913.xyz "KingRumn的Blog")

