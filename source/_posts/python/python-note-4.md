---
title: python学习笔记（四）---高级OOP
date: 2018-08-01 22:22:31
categories:
	- python
tags:
	- python
---

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

__author__ = "Kingrumn"

'''
    高级OOP
    __xx__ 特殊属性的用法
    枚举类
    元类
    多重继承

'''

from enum import Enum, unique


# 正常情况下，当我们定义了一个class，创建了一个class的实例后，我们可以给该实例绑定任何属性和方法
# 动态绑定允许我们在程序运行的过程中动态给class加上功能


# 使用__slots__，可以限制实例的属性
# __slots__定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：
class StudentSlot(object):
    # 只允许实例添加name和age属性。
    __slots__ = ('name', 'age')  # 用tuple定义允许绑定的属性名称


lily = StudentSlot()
lily.age = 19
lily.name = "Lily"


# lily.score = 89     # AttributeError: 'StudentSlot' object has no attribute 'score'


# 利用@property装饰器把一个方法变成属性调用
class StudentProp(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value


jim = StudentProp()
jim.score = 60
print(jim.score)


# jim.score = 9999    # ValueError: score must between 0 ~ 100!


# 定义只读属性，只定义getter方法，不定义setter方法就是一个只读属性
class StudentRd(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):  # birth是可读写属性，而age就是一个只读属性，因为age可以根据birth和当前时间计算出来
        return 2015 - self._birth


# 类的继承可能分为多个维度
# 动物---哺乳动物，鸟类；
# 动物---会跑的动物，会飞的动物；
# 动物---宠物，非宠物
# 通过多重继承，一个子类就可以同时获得多个父类的所有功能
# 这种设计通常称之为MixIn

class Animal(object):
    pass


# 大类:
class Mammal(Animal):
    pass


class Bird(Animal):
    pass


# 大类
class Runnable(object):
    def run(self):
        print('Running...')


class Flyable(object):
    def fly(self):
        print('Flying...')


# 具体动物类
class Dog(Mammal, Runnable):
    pass


class Bat(Mammal, Flyable):
    pass


# 类中__xx__的特殊作用
# __slots__     用于限制属性
# __len__()     使类可用于len()方法
# __str__()     打印实例的显示方法, __repr__ 是为调试服务的，作用同
class StudentStr(object):
    def __init__(self, name):
        self.name = name

    def __str__(self):  # 用于print打印
        return 'Student object (name: %s)' % self.name

    __repr__ = __str__  # 用于命令行打印


print(StudentStr('Michael'))  # Student object (name: Michael)


# __iter__      提供使用for...in的能力
class FibIter(object):
    def __init__(self):
        self.a, self.b = 0, 1  # 初始化两个计数器a，b

    def __iter__(self):
        return self  # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b  # 计算下一个值
        if self.a > 100000:  # 退出循环的条件
            raise StopIteration()
        return self.a  # 返回下一个值


# __getitem__       提供下标访问的能力
class FibGetitem(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b
        return a


FibGetitem()[5]


# 使fib可以用切片
class FibSlice(object):
    def __getitem__(self, n):
        if isinstance(n, int):  # n是索引
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a
        if isinstance(n, slice):  # n是切片
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            ll = []
            for x in range(stop):
                if x >= start:
                    ll.append(a)
                a, b = b, a + b
            return ll


f = FibSlice()
print(f[0:5])


# __getattr__   控制访问属性的行为
class StudentGetAttr(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        if attr == 'score':
            return 99


s = StudentGetAttr()
print(s.score)  # 99


# 利用getattr实现链式调用
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__


print(Chain().status.user.timeline.list)  # '/status/user/timeline/list'


# __call__  提供了直接调用实例的能力
class StudentCall(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)


s = StudentCall('Michael')
s()  # My name is Michael.

# 使用枚举类
# from enum import Enum
Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))

# 可以枚举所有成员,value属性则是自动赋给成员的int常量，默认从1开始计数。
for name, member in Month.__members__.items():
    print(name, '=>', member, ',', member.value)  # Jan => Month.Jan , 1 ...

# 也可以直接访问
print(Month.Jan)  # Month.Jan


# 从Enum派生出自定义类，更精确地控制枚举类型
# from enum import Enum, unique
@unique  # @unique装饰器可以帮助我们检查保证没有重复值
class Weekday(Enum):
    Sun = 0  # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6


# 访问方法
print(Weekday.Tue)
print(Weekday['Tue'])
print(Weekday(1))

# 元类---ToDo
```

