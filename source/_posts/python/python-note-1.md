---
title: python学习笔记（一）---基础知识
date: 2018-08-01 17:03:38
categories:
	- python
tags:
	- python
---



*依据廖雪峰官方网站的python教程整理* 

https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000



```python
#!/usr/bin/env python3      # 首行必须指明所需要的解释器，否则无法通过./xx.py的方式执行
# -*- coding: utf-8 -*-     # 指明了编码格式，否则中文无法正常输出
# 文件名【字母/数字/下划线】.py

# import higher


# 数据类型
def lrn_basic():
    # 输出Python的版本
    import platform
    print(platform.python_version())

    import sys
    print(sys.version)
    print(sys.version_info)

    # python3 必须括号
    print("Hello World.")
    print("你好，Python.")

    # 依次打印每个字符串，遇到，会输出一个空格
    print('The quick brown fox', 'jumps over', 'the lazy dog.')
    # 可打印计算结果
    print('100 + 200 = ', 100 + 200)

    # input()输入
    print('请输入你的名字： ')
    name = input()
    print('Hello, ', name)

    # 以#作为注释
    # 缩进4个空格
    # 大小写敏感

    # 整数
    # 可处理任意大小的整数， 0x表示16进制
    print(1)
    print(2000000)
    print(-8876544)
    # 浮点数
    print(1.23e9)
    print(3.24e-19)

    # 字符串，以单引号或双引号括起来，互相嵌套，\转义
    print("I'm OK, \"Tommy\".")
    # 以r''表示内部的内容不转义
    print(r"\\\\ok")
    # 以'''...'''表示多行内容
    print(''' first line.
    second line.
    third line.''')

    # 布尔值 True/False, and/or/not
    print(True and False)

    # 空值 None, 并非0,0是有意义的
    print(None)

    # 变量
    # 动态语言类型，变量类型无需定义
    a = 'ABBC'
    b = a
    a = 'XYZ'
    print(b)    # ABBC
    print(a)    # XYZ

    # 常量
    pi = 3.14159
    print(pi)

    # 地板除和除法
    print(10 / 3)  # 3.333333
    print(10 // 3)  # 3
    print(-10 // 3)  # 取比之小的最大整数 -4

    # 运算符
    '''
        算术运算符：+   -   *   /   %(取余)   **(幂次)  //(向下取整除)
        比较运算符：==    !=  <>  >   <   >=  <=  
        逻辑运算符：and   or  not
        赋值运算符：=     +=  -=  *=  /=  %=  **=     //=
        位运算符：   &   |   ^   ~   <<      >>
        成员运算符：  in      not in
        身份运算符： is       is not        
        
    '''
    ll = "a"
    ls = ["a", "b", "c"]
    print(ll in ls)
    # is 用于判断两个变量引用对象是否为同一个， == 用于判断引用变量的值是否相等。
    a = [1, 2, 3, 4]
    b = a
    print(b is a)       # True
    print(b == a)       # True
    b = a[:]
    print(b is a)       # False
    print(b == a)       # True


# 字符串和编码
def lrn_str():
    print("Output for string")
    '''
    不可变对象
    '''
    # 单行字符串可用"或'表示
    i = "Hello world."
    print(i)
    print("Hello world")

    # 转义字符不计入字符串内
    print("I'm OK, \\Tom\\")

    # 字符串前加r表示原始字符串，不需要转义
    print(r"I'm OK, 'Lily', \\Tom\\")

    # 字符串前加u表示Unicode编码
    print(u"你好")
    '''
        %d 整数
        %s 字符串
        %f 浮点数
        %x 十六进制数字
    '''
    print("I'm %s" % "Hello world")
    print("I'm {0}, want to {1} ！" .format("小明", "say hello world"))


# 列表
def lrn_list():
    print("output for list")
    classes = ["T1", "T2", "T3"]
    # 长度
    print(len(classes))                 # 3
    # 索引,索引可正可负，-1从最后一个元素开始
    print(classes[-1], classes[0])      # T3 T1
    # print(classes[3])                   # IndexError: list index out of range

    # 增删改
    classes.insert(1, "IOT")
    print(classes)                      # ['T1', 'IOT', 'T2', 'T3']
    classes.pop(1)
    print(classes)                      # ['T1', 'T2', 'T3']
    classes[1] = "IOT"
    print(classes)                      # ['T1', 'IOT', 'T3']

    # 元素可可以不同类型，可以为list
    s = ["Tom", 1, True]
    print(s)                            # ['Tom', 1, True]
    s = ["Tom", ["ada", 12, False], True]
    print(s)                            # ['Tom', ['ada', 12, False], True]


# 元组
def lrn_tuple():
    print("Output for tuple.")

    # tuple类似list，一旦初始化后不可修改
    groups = ()  # 空tuple
    print(groups)               # ()

    # 定义tuple
    groups = ("T1", "T2", "T3")
    print(groups)               # ('T1', 'T2', 'T3')

    # 要定义只有一个元素的tuple，必须加(,)，同样的，输出时，print也会加
    groups = (1,)
    print(groups)               # (1,)
    # 下面的情况会被解析为小括号，而不是tuple
    groups = (1)
    print(groups)               # 1

    # tuple中包含list
    groups = ("T1", ["IOT", "WIFI"], "T2")
    print(groups)               # ('T1', ['IOT', 'WIFI'], 'T2')
    groups[1].insert(1, "NETWORK")
    print(groups)              # ('T1', ['IOT', 'NETWORK', 'WIFI'], 'T2')


def lrn_condition():
    age = 20
    if age > 28:
        print("age is %d" % age)
        print("more than 28")
    elif age > 10:
        print("age is %d" % age)
        print("more than ten")
    else:
        print("age is %d" % age)
        print("less than ten")


# 输入输出
def lrn_io():
    age_s = input("birth: ")      # input返回一个string类型的值
    age = int(age_s)
    print("age is %d" % age)


# 循环
def lrn_loop():
    # for...in循环，遍历list/tuple中的内容
    groups = ("T1", "T2", "T3")
    for group in groups:
        print(group)

    # range 函数生成0-x的数字序列， 不包括x
    # list可强制转换为list
    # 计算0-100的加和
    s = 0
    nums = list(range(100))
    for num in nums:
        s += num
    print(s)

    # while 循环
    s = 0
    index = 0
    while index < 200:
        index += 1            # 如果放到continue的后面，灾难就发生了
        if index == 50:
            continue        # continue跳过当次循环, 总和要减去50，index=50没有执行加和
        s += index
        if index >= 100:
            break           # break跳出当前循环，后续条件都不再执行
    print(s)


# 使用字典
def lrn_dict():
    print("Output for dict")
    '''
    在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度
    dict与list相比，有以下特点：
    1. 查找和插入的速度极快，不会随着key的增加而变慢；
    2. 需要占用大量的内存，内存浪费多；
    3. dict内部存放的顺序和key放入的顺序没有关系
    '''

    # 直接以key-value形式定义一个字典
    d = {'T1': 7, 'T2': 12, "T3": 11, 'IOT': 6}
    print(d, d['T2'])       # {'T1': 7, 'T2': 12, 'T3': 11, 'IOT': 6} 12

    # 以key=value的形式追加定义
    d["WIFI"] = 34
    print(d, d['WIFI'])       # {'T1': 7, 'T2': 12, 'T3': 11, 'IOT': 6, 'WIFI': 34} 34

    # 避免key不存在的方法
    # 先判断后使用
    if 'WIFI' in d:
        print(d['WIFI'])
    # 使用dict的get方法
    print(d.get("NETWORK"))       # 如果不存在，返回None
    print(d.get("NETWORK", -1))   # 如果不存在，返回-1， 此处的值可以自定义


def lrn_set():
    print("Output for  set")
    '''
    set和dict类似，也是一组key的集合，但不存储value,key不能重复
    '''
    # 定义一个set需要一个list
    s = set([1, 2, 3, 4])               # {1, 2, 3, 4}
    print(s)
    s = set([1, 2, 2, 3, 4])            # {1, 2, 3, 4}
    print(s)
    s.add(4)
    print(s)                            # {1, 2, 3, 4}
    s.remove(4)
    print(s)                            # {1, 2, 3}


# 函数定义
def lrn_func():
    print("Output for func")
    '''
        定义函数以def开头，括号后写形参
        参数个数不一致会报错，但参数类型不一致需要自行检查
        不写return相当于return None，可简写为return
        
    '''

    # 参数传递
    def my_abs(x):
        if not isinstance(x, (float, int)):             # 检查参数类型
            raise TypeError("bad operand type")      # 触发参数类型错误
        if x >= 0:
            return x
        else:
            return -x
    print(my_abs(12))
    print(my_abs(-1))
    # my_abs('a')

    # 空函数
    def my_empty():
        pass                # pass什么都不做，保证函数可以跑通，在最初定架构的时候可以用

    my_empty()

    # 返回多个值
    def my_multi_value():
        fir = "first arg"
        sec = "second arg"
        return fir, sec
    a, b = my_multi_value()
    print(a, b)

    # 位置参数(x)
    def product(x):
        return x * x

    print(product(2))       # 2*2 =  4

    # 默认参数
    # 必选参数在前，默认参数在后
    # 当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面
    # 默认参数必须指向不变对象
    def product(x, n=2):
        s = 1
        while 0 < n:
            s *= x
            n -= 1
        return s
    print("2*2 = ", product(2))                 # 2*2 =  4
    print("power(2, 3) = ", product(2, 3))       # power(2, 3) =  8

    # 可变参数*numbers，函数内部得到的是个tuple
    def calc(*nbs):
        my_s = 0
        for n in nbs:
            my_s = my_s + n
        return my_s

    print("1+2+3 = ", calc(1, 2, 3))        # 1+2+3 =  6
    print("1+2 = ", calc(1, 2))             # 1+2 =  3
    numbers = [1, 2, 3]
    print("1+2+3 = ", calc(*numbers))       # 1+2+3 =  6

    # 关键字参数
    # 允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict
    def person(name, age, **kw):    # name, age 为位置参数， kw为关键字参数
        print("name: ", name, "age: ", age, "other: ", kw)
    person("Tom", 15)                                               # name:  Tom age:  15 other:  {}
    # name:  Tom age:  15 other:  {'city': 'Shanghai', 'job': 'Engineer'}
    person("Tom", 15, city="Shanghai", job="Engineer")
    my_kw = {'city': 'Shanghai', 'job': 'Engineer'}
    # name:  Tom age:  15 other:  {'city': 'Shanghai', 'job': 'Engineer'}
    person("Tom", 15, **my_kw)

    # 命名关键字参数
    # 关键字参数定义后，调用者仍可以传入不受限制的关键字参数
    # 如果要限制关键字参数的名字，则使用命名关键字参数
    # 需要一个特殊分隔符*， 其后的部分均被视为命名关键字
    def person(name, age, *, city="Shanghai", job="Engineer"):
        # name, age 为位置参数， city,job 为命名关键字参数,只接受city和job作为关键字参数
        # 如果不定义默认值，则city和job为none
        print(name, age, city, job)

    person("Tom", 15, city="Shanghai", job="Engineer")
    person("Tom", 15)

    # 如果定义了可变参数，则不需要*分隔符
    def person(name, age, *args, city="Shanghai", job="Engineer"):
        print(name, age, args, city, job)

    # Tom 15 ('beijing', 'worker') Shanghai Engineer
    person("Tom", 15, "beijing", "worker", city="Shanghai", job="Engineer")

    # 组合参数
    # 参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数
    def f1(x, y, z=0, *args, **kw):
        print('x =', x, 'y =', y, 'z =', z, 'args =', args, 'kw =', kw)

    f1(1, 2, 3, 'a', 'b')       # a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
    my_args = (1, 2, 3, 4)
    my_kw = {'d': 99, 'x': '#'}
    f1(*my_args, **my_kw)             # a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}

    # 递归函数
    # 函数内部调用自身
    # 使用递归函数需要注意防止栈溢出
    def my_sum(n):
        if 1 >= n:
            return 1
        return sum(n-1) + n

    print("1+2+3 = ", my_sum(3))   # 1+2+3 =  6

    # 解决递归调用栈溢出的方法是通过尾递归优化
    # 尾递归是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式
    # 这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况
    def my_sum(n):
        return sum_iter(n, 1)

    def sum_iter(x, y):
        if 1 >= x:
            return y
        return sum_iter(x-1, x+y)

    print("1+2+3 = ", my_sum(3))   # 1+2+3 =  6


# usage
def lrn_usage():
    print("%8s  %11s  %11s" % (u"序号", u"名称", u"说明"))
    index = 0
    for func in funcs:
        print("%8s  %11s  %11s" % (index, func[0], func[2]))
        index += 1
    return


funcs = [
    ["basic",      lrn_basic,          "基础语法"],
    ["str",      lrn_str,          "字符串"],
    ["list",        lrn_list,         "列表"],
    ["tuple",       lrn_tuple,        "元组"],
    ["condition",   lrn_condition,    "条件判断"],
    ["loop",        lrn_loop,        "循环"],
    ["dict",        lrn_dict,       "字典"],
    ["function",    lrn_func,        "函数"],
    # ["highorder_list",    higher.lrn_higher_list,        "高阶特性"],
    # ["highorder_func",    higher.lrn_higher_func,        "高阶函数"],
    ["usage",       lrn_usage,       "用法说明"]
]


# main
if __name__ == "__main__":
    cname = ""
    lrn_usage()
    while cname != "end":
        cname = input(u"请输入所需测试的知识点序号 or end-结束，help-帮助： ")
        if "help" == cname:
            lrn_usage()
            continue
        elif "end" == cname:
            break
        cno = int(cname)
        if cno < len(funcs) - 1:
            print("%s  %11s  %11s \n" % (cno, funcs[cno][0], funcs[cno][2]))
            funcs[cno][1]()
        else:
            print(u"请输入正确的序号")
            lrn_usage()
    print("exit normally")
```



