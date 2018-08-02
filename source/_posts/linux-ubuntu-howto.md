---
title: Ubuntu 使用记录
date: 2018-04-25 22:37
categories:
- ubuntu
tags:
- linux
- ubuntu
---


安装Ubuntu操作系统是个费事费力的活，把一些基本的记录放在这里，便于以后参考


#背景
```
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.4 LTS"
```

# 切换软件源
-  备份并替换/etc/apt/sources.list的源内容(/etc/apt/sources.list)：
```
$ cd /etc/apt
$ sudo mv sources.list sources.list.bak
$ sudo vi sources.list
```
 - 将下面的内容复制到文件中，并保存
```
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
##测试版源
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
# 源码
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
##测试版源
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
# Canonical 合作伙伴和附加
deb http://archive.canonical.com/ubuntu/ xenial partner
deb http://extras.ubuntu.com/ubuntu/ xenial main
```
- 主动更新一下程序
```
$ sudo apt-get update
$ sudo apt-get upgrade
```

# 安装搜狗输入法

基本上分三步走
- 添加fcitx的键盘输入法系统，因为sogou是基于fcitx的，而系统默认的是iBus；
- 安装sogou输入法；
- 设置系统参数及一些注意点。

## 添加fcitx键盘输入法系统
- 先添加以下源
```
$ sudo add-apt-repository ppa:fcitx-team/nightly
```
- 添加源之后需要更新一下系统
```
$ sudo apt-get update
$ sudo apt-get upgrade
```
- 开始安装fcitx
```
$ sudo apt-get install fcitx
```
- 接着安装fcitx的配置工具
```
$ sudo apt-get install fcitx-config-gtk
```
- 然后安装fcitx的table-all软件包
```
$ sudo apt-get install fcitx-table-all
```
- 安装im-switch切换工具
```
$ sudo apt-get install im-switch
```
- 看看我们的fcitx是否安装完成
搜索fcitx查看是否有安装成功,其实fcitx只是一个输入法工具,本身不带资源包的,所以Ubuntu下安装搜狗输入法，需要我们自己来下载搜狗输入法的资源包。

## 安装sogou输入法
- 选择你操作系统的版本下载就可以了`https://pinyin.sogou.com/linux/?r=pinyin`
- 用dpkg命令来安装搜狗输入法资源包，出现错误可暂时不用管

## 设置语言选项
- 到系统设置->语言支持，将键盘输入法系统由默认的iBus设置为fcitx
- 这个时候是看不到效果的，一定要注销一次
- 搜索出fcitx配置，将sogou输入法设为默认即可
sogou-qimpanel 命令是启动搜狗输入法的。
- 如果还是切换不出输入法，那么执行下面的操作：
```
#配置Fcitx的环境:在~/.bashrc中加入一下内容
export GTK_IM_MODULE=fcitx 
export QT_IM_MODULE=fcitx 
export XMODIFIERS="@im=fcitx" 
```
## 问题总结
另外，在安装过程中遇到好几次这个问题，在网上查找到方法解决掉了，总结一下：

- Could not get lock /var/lib/dpkg/lock
	通过终端安装程序sudo apt-get install xxx时出错：
```
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```
出现这个问题可能是有另外一个程序正在运行，导致资源被锁不可用。而导致资源被锁的原因可能是上次运行安装或更新时没有正常完成，进而出现此状况，解决的办法其实很简单：
```
#在终端中敲入以下两句
$ sudo rm /var/cache/apt/archives/lock
$ sudo rm /var/lib/dpkg/lock
```
再试着安装，问题解决。


# 修改DNSserver
```
$ sudo vi /etc/resolvconf/resolv.conf.d/base
```
修改内容：
```
search localdomain #如果本Server为DNS服务器，可以加上这一句，如果不是，可以不加
nameserver 8.8.8.8 #希望修改成的DNS
nameserver 114.114.114.114 #希望修改成的DNS
```
保存退出，重启网络：
```
$ sudo /etc/init.d/networking restart
```
查看当前DNS：
```
$ cat /etc/resolv.conf
```



# Sublime 安装及激活

## 安装

 到官方网站上(`http://www.sublimetext.com/3`)下载64位（系统位64位）的.deb安装包（`http://c758482.r82.cf2.rackcdn.com/sublime-text_build-3059_amd64.deb`）

```
$ wget http://c758482.r82.cf2.rackcdn.com/sublime-text_build-3059_amd64.deb
$ sudo dpkg -i sublime-text_build-3059_amd64.deb
```

## 激活

help >>  enter Licence
```
Nicolas Hennion
Single User License
EA7E-866075
8A01AA83 1D668D24 4484AEBC 3B04512C
827B0DE5 69E9B07A A39ACCC0 F95F5410
729D5639 4C37CECB B2522FB3 8D37FDC1
72899363 BBA441AC A5F47F08 6CD3B3FE
CEFB3783 B2E1BA96 71AAF7B4 AFB61B1D
0CC513E7 52FF2333 9F726D2C CDE53B4A
810C0D4F E1F419A3 CDA0832B 8440565A
35BF00F6 4CA9F869 ED10E245 469C233E
```
以上激活码可用



# Typora 安装

typora 是当前非常通用的markdown编辑软件，官网`https://typora.io`

```
# optional, but recommended
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

# add Typora's repository
$ sudo add-apt-repository 'deb https://typora.io linux/'

sudo apt-get update

# install typora
$ sudo apt-get install typora
```

PS： remarkble这个软件就是个shit，千万不要用

\n\n 更多更及时的博客更新请戳--->  [KingRumn](http://zm913.xyz "KingRumn的Blog")\n
