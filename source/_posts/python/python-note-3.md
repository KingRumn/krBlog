---
title: python学习笔记（三）---类与多态
date: 2018-08-01 21:17:31
categories:
	- python
tags:
	- python
---

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

r'''
    类： 类和实例，访问控制，继承、多态
'''

__author__ = "Kingrumn"

import types

# 定义类
# 通过class关键字定义类
# class后面紧接着是类名，即Student，类名通常是大写开头的单词
# (object)，表示该类是从哪个类继承下来的, 如果没有合适的类继承，用object总是没错的
class Student1(object):
    pass


# 创建实例
# 通过类名+()创建实例
stu = Student1()
print(stu)      # <__main__.Student object at 0x0145AEB0>


class Student2(object):
    # 通过定义一个特殊的__init__方法，在创建实例的时候，就把name，score等属性绑上去
    def __init__(self, name, age):
        # 第一个参数永远是self，表示创建的实例本身
        self.name = name
        self.age = age

    # 和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量self，
    # 调用时，不用传递该参数
    # 除此之外，类的方法和普通函数没有什么区别
    def print_age(self):
        print(self.age)

    # 给类增加新的方法
    def get_grade(self):
        if self.age > 18:
            return "大学"
        else:
            return "中学"


# 数据封装
# 同样的，可以在类外部通过封装定义print_age函数
def print_age(s):
    print(s.age)


# 增加__init__方法后，在创建实例的时候，就不能传入空的参数了，必须传入与__init__方法匹配的参数，但self不需要传
tom = Student2("Tom", 15)
print(tom.age, tom.name)        # 15 Tom
tom.print_age()         # 15
print_age(tom)          # 15

jack = Student2("Jack", 19)
print(tom.name, tom.get_grade())    # Tom 中学
print(jack.name, jack.get_grade())  # Jack 大学


# 访问限制
# 通过添加__使属性变为私有属性，外部不可访问
class Student3(object):
    # 属性的名称前加上两个下划线__, 则变为私有属性
    def __init__(self, name, age):
        self.__name = name
        self.__age = age

    # 属性私有后，需要提供访问接口
    def get_age(self):
        return self.__age

    def set_age(self, age):
        self.__age = age

    def get_name(self):
        return self.__name

    def set_name(self, name):
        self.__name = name

    # 通过提供方法提供访问私有属性的能力
    def get_grade(self):
        if self.__age > 18:
            return "大学"
        else:
            return "中学"


# 内部可以访问，外部不可以访问
jack = Student3("Jack", 19)
print(jack.get_grade())  # 大学
# print(jack.__name)      # AttributeError: 'Student3' object has no attribute '__name'
jack.set_name("Jacky")
jack.set_age(15)
print(jack.get_name(), jack.get_age())  # Jacky 15

# 在Python中，变量名类似__xxx__的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，
# 特殊变量是可以直接访问的，不是private变量，所以，不能用__name__、__score__这样的变量名

# 有些时候，你会看到以一个下划线开头的实例变量名，比如_name，
# 这样的实例变量外部是可以访问的，
# 但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”

# 一种错误的认知
jack.__name = "xxx"
print(jack.get_name(), jack.__name)     # Jacky xxx
# 你会发现__name并非类的属性，虽然赋值是成功的，但是这是完全不同的2个东西


# 继承和多态
class Animal(object):
    def run(self):
        print('Animal is running...')


class Dog(Animal):
    pass


class Cat(Animal):
    # 也可以对子类增加一些方法，比如Dog类：
    def run(self):
        # 子类的run方法覆盖了父类的run方法
        print('Cat is running')

    def eat(self):
        print('Cat is Eating meat...')


# 上例中，Animal是Dog和Cat的父类、基类、超类；Dog和Cat是Animal的子类
# 子类获得了父类的全部功能
dog = Dog()
dog.run()       # Animal is running...
cat = Cat()
cat.run()       # Cat is running
cat.eat()       # Cat is Eating meat...


# 多态定义
def who_run(animal):
    animal.run()


# dog和cat都是animal，所以都可以传进来
dog.run()       # Animal is running...
cat.run()       # Cat is running
# 著名的“开闭”原则
# 对扩展开放：允许新增Animal子类；
# 对修改封闭：不需要修改依赖Animal类型的who_run()等函数


# 对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了
# 对于静态语言（例如Java）来说，如果需要传入Animal类型，则传入的对象必须是Animal类型或者它的子类，否则，将无法调用run()方法
class Wind(object):
    def run(self):
        print("wind is running...")


wind = Wind()
# 只要wind定义了run方法就可以，不需要一定是Animal类或者其子类
who_run(wind)   # wind is running...

# 获取对象信息
# 使用type()
print(type(wind))   # <class '__main__.Wind'>

# 基本数据类型，int/str/bool/float
# 类型常量：import types：types.FunctionType、types.BuiltinFunctionType、types.LambdaType、types.GeneratorType
print(type((x * x for x in range(10))) == types.GeneratorType)  # True, 不推荐这样使用，可使用isinstance()

# 使用isinstance(), 总是优先使用isinstance()判断类型，可以将指定类型及其子类“一网打尽”。
print(isinstance(dog, Dog))     # True
print(isinstance(dog, Animal))  # True

# 使用dir()
# 获得一个对象的所有属性和方法, 它返回一个包含字符串的list
print(dir(tom))
# [
#   '_Student3__age', '_Student3__name',
#   '__class__', '__delattr__',
#   '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__',
#   '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__',
#   '__le__', '__lt__', '__module__', '__name', '__ne__', '__new__', '__reduce__',
#   '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
#   '__weakref__',
#   'get_age', 'get_grade', 'get_name', 'set_age', 'set_name'
# ]

# 配合getattr()、setattr()以及hasattr()，我们可以直接操作一个对象的状态
print(hasattr(tom, '__name'))       # False
setattr(tom, 'score', 19)
print(getattr(tom, 'score'))        # 19

# 实例属性和类属性
# 给实例绑定属性的方法是通过实例变量，或者通过self变量：
jack.instance_attr = 58     # 实例属性，其他实例是无法访问的
jack.get_name()         # 访问的是类属性，所有实例均可以访问

```

