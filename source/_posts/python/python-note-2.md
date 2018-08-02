---
title: python学习笔记（二）---高级特性
date: 2018-08-01 19:01:09
categories:
	- python
tags:
	- python
---

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

r'''
    高阶知识
    lrn_higher_list： 切片、迭代、列表生成式、生成器、迭代器
    lrn_higher_func： 函数式编程，修饰器、匿名函数、筛选器等
'''

__author__ = 'Kingrumn'

'''
    第1行和第2行是标准注释，第1行注释可以让这个.py文件直接在Unix/Linux/Mac上运行，第2行注释表示.py文件本身使用标准UTF-8编码；

    第4行是一个字符串，表示模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释；

    第10行使用__author__变量把作者写进去，这样当你公开源代码后别人就可以瞻仰你的大名；
'''


from collections import Iterable
from collections import Iterator
from functools import reduce
import functools


# 高级特性--切片、迭代、列表生成式、生成器、迭代器
def lrn_higher_list():
    # 切片Slice
    lst = list(range(100))

    # 取前N个元素，也就是索引为0-(N-1)的元素
    # list[a:b:c]: c>0, a<b; c<0, a>b, >指位置靠后
    print(lst[0:3])      # [0, 1, 2]
    print(lst[:3])      # [0, 1, 2]

    # 后N个元素
    print(lst[-2:])     # [98, 99]

    # 带步长取
    print(lst[10:20:2])     # [10, 12, 14, 16, 18]
    print(lst[::15])     # [0, 15, 30, 45, 60, 75, 90]

    # 逆序取列表
    print(lst[20:10:-2])    # [20, 18, 16, 14, 12]
    print(lst[-10:80:-2])    # [20, 18, 16, 14, 12]

    # 迭代
    # dict迭代方法
    d = {"a": 1, "b": 2, "c": 3}
    # 按key迭代
    for key in d:
        print(d[key])
    # 按value迭代
    for value in d.values():
        print(value)
    # 同时迭代
    for k, v in d.items():
        print(k, v)
    # 判断对象是否可迭代
    # from collections import Iterable
    print(isinstance('abc', Iterable))
    # enumerate可将list变为索引-元素对
    for k, v in enumerate(['a', 'b', 'c']):
        print(k, v)

    # 列表生成式List Comprehensions
    # range:    range(start, stop[, step])
    print(list(range(1, 11, 2)))            # [1, 3, 5, 7, 9]
    print([x * x for x in range(1, 5)])     # [1, 4, 9, 16]
    print([x * x for x in range(1, 11) if x % 2 == 0])  # [4, 16, 36, 64, 100]
    print([m + n for m in 'ABC' for n in 'XYZ'])        # ['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
    d = {'x': '1', 'y': '2'}
    print([k + '=' + v for k, v in d.items()])      # ['x=1', 'y=2']
    lst = ['A', 'B', 'C']
    print([s.lower() for s in lst])           # ['a', 'b', 'c']

    # 生成器
    # 在循环的过程中不断推算出后续的元素，不必创建完整的list
    # 一边循环一边计算的机制，称为生成器：generator
    # 方法一：列表生成式的[]改成()
    lst = (x * x for x in range(10))
    print(lst)          # <generator object lrn_higher.<locals>.<genexpr> at 0x0322C8B0>
    print(next(lst))    # 0
    print(next(lst))    # 1
    print(next(lst))    # 4
    # 没有更多的元素时，抛出StopIteration的错误

    # generator也是可迭代对象, 用for循环才是正确的做法
    for n in lst:
        print(n)

    # 方法二：利用yield 构造；
    # 一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator
    # 函数是顺序执行，遇到return语句或者最后一行函数语句就返回；
    # 而变成generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行；
    # 用for循环调用generator时，发现拿不到generator的return语句的返回值，
    # 如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中
    # 斐波那契数列
    def fib(m):
        n, a, b = 0, 0, 1
        while n < m:
            yield b
            a, b = b, a + b
            n = n + 1
        return 'done'
    next(fib(10))

    # 杨辉三角
    print(u"杨辉三角")

    def yt():
        a = [1]
        yield a
        b = [1, 1]
        yield b
        while True:
            index = 0
            a = b
            b = [1, 1]
            while index < len(a) - 1:
                at = a[index] + a[index+1]
                b.insert(-1, at)
                index += 1
            yield b

    n = 0
    results = []
    for t in yt():
        print(t)
        results.append(t)
        n = n + 1
        if n == 10:
            break

    # 迭代器
    # 可以被next()函数调用并不断返回下一个值的对象称为迭代器：Iterator
    # 可以使用isinstance()判断一个对象是否是Iterator对象
    # 生成器都是Iterator对象，但list、dict、str虽然是Iterable，却不是Iterator
    # 可以使用iter()函数把list、dict、str等Iterable变成Iterator
    print(isinstance((x for x in range(10)), Iterator))     # True
    print(isinstance(iter([]), Iterator))               # True
    print(isinstance([], Iterator))             # False


# 函数式编程
def lrn_higher_func():

    def p(x):
        return x

    def pa(x):
        return -x
    # 变量可以指向函数
    f = p(-10)
    print(f)            # -10
    f = p
    print(f)            # <function lrn_function_higher.<locals>.p at 0x036348E8>

    # 函数名也是变量,可以进行赋值，这么做是危险的
    print(pa(-10))      # 10
    pa = p
    print(pa(-10))     # -10

    # 高阶函数
    # 一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数
    def add(x, y, func):
        return func(x) + func(y)

    print(add(-5, 4, abs))  # 9

    # map
    # map()函数接收两个参数，一个是函数，一个是Iterable
    # map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回
    def f(x):
        return x * x
    r = map(f, [1, 2, 3, 4, 5])
    print(list(r))      # [1, 4, 9, 16, 25]
    print(list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9])))      # ['1', '2', '3', '4', '5', '6', '7', '8', '9']

    # reduce
    # reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算
    # from functools import reduce
    def add(a, b):
        return a * 10 + b

    print(reduce(add, [1, 3, 5, 7, 9]))     # 13579

    digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

    def char2num(s):
        return digits[s]

    # str2int 函数： ’13579‘  --->   13579
    def str2int(s):
        def fn(x, y):
            return x * 10 + y

        return reduce(fn, map(char2num, s))
    print(str2int('13579'))     # 13579

    # 用lambda函数简化写法
    def str2int(s):
        return reduce(lambda x, y: x * 10 + y, map(char2num, s))

    print(str2int('13579'))     # 13579

    # str2float
    def str2float(s):
        index = s.find('.')
        return reduce(lambda x, y: x * 10 + y, map(char2num, s[:index])) \
               + reduce(lambda x, y: x * 0.1 + y, map(char2num, s[-1:index:-1]+'0'))
    print(str2float('0.123'))

    # filter
    # filter()也接收一个函数和一个序列
    # filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素
    # filter()函数返回的是一个Iterator
    def is_odd(x):
        return x % 2 == 1

    print(list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15])))     # [1, 5, 9, 15]

    # 利用埃氏筛法求所有素数
    # 依次从第一个筛掉所有能被整除的数
    def odd_iter():
        n = 1
        while True:
            n += 2
            yield n

    def not_divisible(n):
        return lambda x: x % n > 0  # 此处返回的是个函数

    def primes():
        yield 2
        it = odd_iter()  # 初始序列
        while True:
            n = next(it)  # 返回序列的第一个数
            yield n
            it = filter(not_divisible(n), it)  # 构造新序列
    mn = 0
    ml = []
    for i in primes():
        ml.append(i)
        mn += 1
        if mn > 10:
            break
    print(ml)

    # sorted
    # 可以对list进行排序
    # sorted()函数也是一个高阶函数，它还可以接收一个key函数来实现自定义的排序
    '''
         sorted(iterable[, cmp[, key[, reverse]]])，后面的可选参数为命名关键字参数
         iterable -- 可迭代对象。
         cmp -- 比较的函数，这个具有两个参数，参数的值都是从可迭代对象中取出，此函数必须遵守的规则为，大于则返回1，小于则返回-1，等于则返回0
         key - - 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。
         reverse -- 排序规则，reverse = True 降序 ， reverse = False 升序（默认）。
         返回重新排序的列表, 保持原列表不变
    '''
    print(sorted([36, 5, -12, 9, -21], key=abs))

    # 函数作为返回值
    def lazy_sum(*args):
        def s():
            ax = 0
            for n in args:
                ax = ax + n
            return ax

        return s

    f1 = lazy_sum(1, 3, 5, 7, 9)
    f2 = lazy_sum(1, 3, 5, 7, 9)
    print(f1 == f2)     # 此时才真正进行计算

    # 闭包
    # 返回函数不要引用任何循环变量，或者后续会发生变化的变量。
    def count():
        def func(j):
            def g():
                return j * j

            return g

        fs = []
        for i in range(1, 4):
            fs.append(func(i))  # func(i)立刻被执行，因此i的当前值被传入func()
        return fs

    f1, f2, f3 = count()
    print(f1())         # 1
    print(f2())         # 4
    print(f3())         # 9

    # 匿名函数
    # 函数没有名字，不必担心函数名冲突
    # 也可以把匿名函数赋值给一个变量，再利用变量来调用该函数
    def build(x, y):
        return lambda: x * x + y * y

    print(build(1,2)())

    # 装饰器
    # 函数有个内置的__name__属性，可以拿到函数的名字
    # decorator就是一个返回函数的高阶函数
    def log(func):
        @functools.wraps(func)              # functools模块，用于修改wrapper的__name__为func.__name__
        def wrapper(*args, **kw):
            print('call %s():' % func.__name__, end=' ')    # 增加end=''防止换行
            return func(*args, **kw)

        return wrapper

    @log                # 相当于执行了now = log(now)
    def now():
        print('2015-3-25')

    now()       # call now(): 2015-3-25

    # 偏函数
    # 把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单
    # import functools
    # functools.partial
    int2 = functools.partial(int, base=2)
    print(int2('1010101'))         # 85


# main
'''
    当我们在命令行运行hello模块文件时，Python解释器把一个特殊变量__name__置为__main__，
    而如果在其他地方导入该hello模块时，if判断将失败，
    因此，这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试
'''
if __name__ == "__main__":
    lrn_higher_list()
    lrn_higher_func()

```


\n\n 更多更及时的博客更新请戳--->  [KingRumn](http://zm913.xyz "KingRumn的Blog")\n
