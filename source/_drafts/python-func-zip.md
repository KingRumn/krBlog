---
title: python内置函数--zip
categories:
  - python
tags:
  - python
date: 2018-08-28 19:27:15
---





 更多更及时的博客更新请戳--->  [KingRumn](http://zm913.xyz "KingRumn的Blog")



# zip() 

## 描述

用于将一系列可迭代的对象作为参数，将对象中对应的元素打包成一个个元组（tuple），然后返回由这些元组组成对象，其可以通过list函数将其转换为列表。

若各个迭代器的长度不同，则返回长度与最短对象的长度相同。

利用**\***操作符可以将元组对象解压为列表。

> Note： zip 方法在 Python 2 和 Python 3 中的不同：在 Python 2.x zip() 返回的是一个列表。

## 语法

```python
zip(iter1 [,iter2 [...]])
```

## 返回值

返回一个对象

## 举例

```python 3.6
>>> a = [1,2,3]
>>> b = [4,5,6]
>>> c = [4,5,6,7,8]
>>> zipped = zip(a,b)	# 压缩为一个元组，并返回一个对象
>>> zipped
<zip object at 0x0000025CF915C4C8>
>>> list(zipped)	# list转换为列表
[(1, 4), (2, 5), (3, 6)]
>>> list(zip(a,c)) 	# 元素个数与最短对象相同
[(1, 4), (2, 5), (3, 6)]

>>> a1, a2 = zip(*zip(a,b)) 
>>> list(a1)
[1, 2, 3]
>>> list(a2)
[4, 5, 6]
```

## 补充

利用zip实现二维矩阵转置

```python 3.6
>>> a = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
>>> list(zip(*a))
[(1, 4, 7), (2, 5, 8), (3, 6, 9)]
```

