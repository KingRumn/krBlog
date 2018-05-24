---
title: Yolo安装（Ubuntu14.04）
date: 2018-04-25 22:37
categories:
- computer vision
tags:
- opencv
- computer vision
- yolo
---

最近在同事的建议下想要学习一下深度学习的知识，作为环境搭建的第一步，配置Yolo环境。电脑不支持CUDA，该环境仅用来学习，故未对CUDA进行配置和支持。
在环境配置过程中也遇到一些问题，记录以作参考。

## 经验总结
* 做一件事的时候，最基础的是去官网按照说明走一遍，往往这才是最官方的，也是最正确的，出了问题才去其他地方寻找答案
* 如果开源的代码出了问题，不妨先在Github上搜一搜看看有没有人碰到

## 官方参考资料
* Yolo安装指南：`https://pjreddie.com/darknet/install/`
* Joseph Chet Redmon个人网站：`https://pjreddie.com/`
* OpenCV官网：`https://opencv.org/`
* OpenCV安装指南：`https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html`

## 安装OpenCV
当前（2018-3-20）下载的opencv的最新版本应该是3.4.1，下面的安装过程完整参考**OpenCV安装指南**中的说明，如无压力可直接跳过，如果OpenCV已经安装，也请跳过。

### 检查需求
```
GCC 4.4.x or later
CMake 2.8.7 or higher
Git
GTK+2.x or higher, including headers (libgtk2.0-dev)
pkg-config
Python 2.6 or later and Numpy 1.5 or later with developer packages (python-dev, python-numpy)
ffmpeg or libav development packages: libavcodec-dev, libavformat-dev, libswscale-dev
[optional] libtbb2 libtbb-dev
[optional] libdc1394 2.x
[optional] libjpeg-dev, libpng-dev, libtiff-dev, libjasper-dev, libdc1394-22-dev
[optional] CUDA Toolkit 6.5 or higher
```

###安装相关工具
```
[compiler] $ sudo apt-get install build-essential
[required] $ sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
[optional] $ sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
```

###下载
```
#创建工作目录，本文使用的是～目录作为工作目录
$ cd ~/<my_working_directory>
#克隆代码，这个过程应该会持续一段时间
$ git clone https://github.com/opencv/opencv.git
```

###编译
编译目录位置比较随意，我为了保证源码的干净，在与opencv目录下建了build340的目录用来编译，340代表版本是3.4.0（至于为什么后面再说）。
```
#创建编译目录
$ cd ~/opencv/..
$ mkdir build340
$ cd build340
# 生成配置指明了release编译和安装路径，最后的..是CMakeLists.txt文件的相对位置
$ cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
# 编译，-j指明了多核编译的核数，请自行修改，整个过程持续约5到10分钟不等
$ make -j7
# 安装
sudo make install
```

###检查安装结果
使用examples下的demo测试，此方案需要自己进行编译，另外需要一张带有人脸的测试照片
![原始图像](https://upload-images.jianshu.io/upload_images/11077939-ec680e8d2fc13fe0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
#创建编译目录
$ cd ~/opencv/..
$ mkdir buildSamples
$ cd buildSamples
#配置
$ cmake ../opencv/src/samples
#编译
$ make
#测试
$ cd cpp
$ ./cpp-example-facedetect ~/Downloads/timg.jpeg
或
$ ./cpp/cpp-example-facedetect --cascade="../opencv/data/haarcascades/haarcascade_eye_tree_eyeglasses.xml" ../timg.jpeg
```
![检测结果](https://upload-images.jianshu.io/upload_images/11077939-464422dd5d5fb957.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

至此，opencv的安装告一段落，如果想要配置其他功能或测试，请参考官网安装指南。

## 安装darknet
依然参考官网权威的安装指南。

###下载代码
```
$ git clone https://github.com/pjreddie/darknet.git
```

###开启opencv支持
```
$ cd darknet
#修改OPENCV=1
$ vi Makefile
```

###编译
```
$ make
#大概的输出
mkdir -p obj
gcc -I/usr/local/cuda/include/  -Wall -Wfatal-errors  -Ofast....
gcc -I/usr/local/cuda/include/  -Wall -Wfatal-errors  -Ofast....
gcc -I/usr/local/cuda/include/  -Wall -Wfatal-errors  -Ofast....
.....
gcc -I/usr/local/cuda/include/  -Wall -Wfatal-errors  -Ofast -lm....
```
执行完成后，在当前目录下查看编译是否成功：
```
$ ./darknet
usage: ./darknet <function>
```

### 下载配置文件
https://pjreddie.com/darknet/yolo/提供了具体的测试方法，本文为了测试，下载了相对较小的tiny-yolo-voc网络的配置
```
# 下载文件
$ wget https://pjreddie.com/media/files/tiny-yolo.weights
# 测试结果
$ ./darknet detect ./cfg/tiny-yolo.cfg ./../tiny-yolo.weights ./data/dog.jpg
#输出内容
layer     filters    size              input                output
    0 conv     16  3 x 3 / 1   416 x 416 x   3   ->   416 x 416 x  16
    1 max          2 x 2 / 2   416 x 416 x  16   ->   208 x 208 x  16
    2 conv     32  3 x 3 / 1   208 x 208 x  16   ->   208 x 208 x  32
    3 max          2 x 2 / 2   208 x 208 x  32   ->   104 x 104 x  32
    4 conv     64  3 x 3 / 1   104 x 104 x  32   ->   104 x 104 x  64
    5 max          2 x 2 / 2   104 x 104 x  64   ->    52 x  52 x  64
    6 conv    128  3 x 3 / 1    52 x  52 x  64   ->    52 x  52 x 128
    7 max          2 x 2 / 2    52 x  52 x 128   ->    26 x  26 x 128
    8 conv    256  3 x 3 / 1    26 x  26 x 128   ->    26 x  26 x 256
    9 max          2 x 2 / 2    26 x  26 x 256   ->    13 x  13 x 256
   10 conv    512  3 x 3 / 1    13 x  13 x 256   ->    13 x  13 x 512
   11 max          2 x 2 / 1    13 x  13 x 512   ->    13 x  13 x 512
   12 conv   1024  3 x 3 / 1    13 x  13 x 512   ->    13 x  13 x1024
   13 conv    512  3 x 3 / 1    13 x  13 x1024   ->    13 x  13 x 512
   14 conv    425  1 x 1 / 1    13 x  13 x 512   ->    13 x  13 x 425
   15 detection
mask_scale: Using default '1.000000'
Loading weights from ./../tiny-yolo.weights...Done!
./data/dog.jpg: Predicted in 2.440847 seconds.
car: 38%
person: 26%
car: 25%
car: 74%
person: 28%
dog: 82%
bicycle: 59%

```
本文所用的tiny-yolo.weights检测速度比较快，但是精度有限，可以看到图中的检测结果并不准确。
![测试结果](https://upload-images.jianshu.io/upload_images/11077939-aa4f4b50a39f74d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好了，yolo的配置基本就结束了，下面写写碰到的几个坑。

##问题一  编译darknet过程中出现以下错误
```
/usr/include/opencv2/core/cvdef.h:485:1: error: unknown type name 'namespace'
```
这个问题曾经困扰我好久，找了很多地方原因大概都是编译器在尝试用c编译c++的内容，最后，还是在Github的提交里找到了答案（https://github.com/pjreddie/darknet/issues/502），这是opencv 3.4.1的固有问题，大概的原因是opencv只提供标准C++的接口，导致用C编译器去编译的时候可能出问题，有个解决方案但是并没有入库（https://github.com/opencv/opencv/issues/10963）
说回到这个问题，只要将opencv的版本切换回3.4.0就可以了，这就是为什么之前选择3.4.0的原因。
当然，如果是通过```git clone```的代码下来，是不需要再次克隆代码的，只需要执行checkout指令就可以了，这也是为什么编译的时候要在同级目录下创建编译目录以保证源码干净的原因。
```
$ git checkout 3.4.0
```

##问题二  测试darknet时输出的图片没有结果

这个问题同样困扰了好久，网上有几种解决方案，比如将CUDNN开关打开重新编译等等，都没有解决，最终竟然发现仅仅是配置文件和网络权重选择不一致导致的，（大写的囧o(╯□╰)o)。
所以参数3（*.cfg)和参数4(*.weights)一定要选择相对应的文件。
```
$ ./darknet detect ./cfg/tiny-yolo.cfg ./../tiny-yolo.weights ./data/dog.jpg
```

##问题三 找不到某个opencv的库

在ubuntu下编译opencv程序后，执行报下面到错误：
error while loading shared libraries: libopencv_core.so.2.4: cannot open shared object file: No such file or directory

解决方法：找到libopencv_开头到库的目录，在/usr/local/lib下面，在/etc/ld.so.conf.d/下面新建一个opencv.conf，里面写入/usr/local/lib，最后执行下sudo ldconfig -v即可

好了，写到这里，祝顺利。
