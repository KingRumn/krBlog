---
title: 每天一个OpenCV函数----cvAbs/cvAbsDiff/cvAbsDiffS
date: 2018-05-24 22:37
categories:
- computer vision
tags:
- opencv
- computer vision
---



学习OpenCV的过程比较漫长，对一门技术的最好学习时在实践中积累与总结。

从今天起，开启一个每天一个OpenCV函数的系列，积累在学习过程中的思考。

# 功能函数

##cvAbsDiff

```C++
/*modules/core/src/arithm.cpp*/
void cvAbsDiff( 
    const CvArr* srcarr1, 
    const CvArr* srcarr2, 
    CvArr* dstarr 
    );
```

**作用**: 计算```srcarr1 - srcarr2``` 的绝对值，并写入dstarr。

**说明**: 

* 将CvArr转换为CvMat，并利用```cv::absdiff```计算差值；
* ```CV_Assert```保证了src与dst的大小必须一致；

## cvAbsDiffS

```C++
/*modules/core/src/arithm.cpp*/
void cvAbsDiffS( 
    const CvArr* srcarr1, 
    CvArr* dstarr, 
    CvScalar scalar
    );
```

**作用**: 从```src```中减掉标量值。

**说明**: 

- 将CvArr转换为CvMat，并利用```cv::absdiff```计算差值；
- ```CV_Assert```保证了src与dst的大小必须一致；

##  cvAbs

```C++
/*module/coure/include/opencv2/core/core_c.h*/
#define cvAbs( src, dst ) cvAbsDiffS( (src), (dst), cvScalarAll(0))
```

**作用**: 计算```src``` 的绝对值，并写入```dst```。

**说明**: 

- 借助```cvAbsDiffS```完成；

# 通用函数

## CV_Assert

```C++
#define CV_Assert(cond) do { if (!(cond)) abort(); } while (0)
```

**作用**: 断言```cond```的值

**说明**: 

- 如果cond为假，直接退出；

## cv::cvarrToMat
--ToDo


## cv::absdiff

--ToDo