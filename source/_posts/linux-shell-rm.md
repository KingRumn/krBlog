---
title: 修改rm命令以避免误删
date: 2018-05-31 22:37
categories:
- linux
tags:
- shell
- linux
---



#问题重现

是否碰到过如下情况：

* 写了一个月的代码，没有做本地提交，手滑来了个`rm -rf `，然后就没有然后了。
* 并没有注意一个名叫～的文件夹，有一丢丢洁癖的自己习惯性的`rm -rf ～`，一回车，世界突然安静。
* ... ...

网上大概的解决方案都是1. 找个地方，2. rm改为mv， 实现rm时把文件直接mv到这个地方，3. 提供清空回收站/查看回收站文件/找回误删文件等的操作。

# 基本需求

基本功能参考windows中的回收站。

1. 准备一个文件夹，作为垃圾回收站（~/.trash);
2. 删除文件：轻松将需要删除的文件移动到回收站中，支持文件夹及文件；支持绝对路径和相对路径；支持重名文件；
3. 清空回收站：彻底删除回收站中的文件及相关资源；
4. 查看回收站文件列表：罗列回收站中的文件，罗列回收站中的文件的恢复路径；
5. 恢复某个文件：按照恢复路径及原始文件名称恢复文件，该命令应具有自动补全功能；
6. 重构rm命令，替换其功能；
7. 提供简洁的命令访问；

# 开发过程

## 准备全局变量

```shell
export HOME_TRASH_PATH=~/.trash		# 回收站的位置
export TRASH_FILE_LIST=$HOME_TRASH_PATH/.list	# 用于存放原始文件的路径
# 创建回收站
if [ ! -d "$HOME_TRASH_PATH" ]; then
   mkdir -p "$HOME_TRASH_PATH"
fi

# 创建回收记录
if [ ! -f "$TRASH_FILE_LIST" ]; then
   touch "$TRASH_FILE_LIST"
fi
```

`HOME_TRASH_PATH`提供了回收站的位置，该功能只在当前用户环境下生效，故采用～目录；避免不同用户使用可能存在的权限问题。

`TRASH_FILE_LIST`文件为文本文件，用于存放原始文件的路径，为恢复文件提供依据；

## 删除文件

```shell
function _trash_delete()
{
   local trash_path=$HOME_TRASH_PATH
   local trash_nanme
   local file_name
   local file_path
   
   # 应支持多个文件(夹)一起删除
   for tmp_path in $@;
   do
      # 从参数中分离出文件路径和文件名
      # 利用IFS分割Unix风格的文件路径
      local old_ifs="$IFS"
      IFS="/"
      local file_array=($tmp_path)
      IFS="$old_ifs"
      
      # 数组中最后一个为文件名, 注意冒号后有个空格
      local length=${#file_array[@]}
      file_name=${file_array[@]: -1}

      # 指定文件名为"名.时间",避免重名，同时日期可以提供找回的参考
      trash_name=$file_name.`date +%Y%m%d-%H:%M:%S`

      # 获取第一个字符确认是否为绝对路径
      local _prefix=${tmp_path:0:1}
      if [ "/" = $_prefix -o "~" = $_prefix ]; then
         # 绝对路径只需要去掉文件名就可以获得路径，eg:/home/aaa/test_dir
         file_path=${tmp_path%/$file_name*}
      else
         # 相对路径需要解析出部分路径后与当前路径叠加
         file_path=${tmp_path%/$file_name*}
         file_path=`pwd`"/$file_path"
      fi
      # 将路径写入TRASH_FILE_LIST文件
      echo -e "$trash_name \t\t $file_path" >> $TRASH_FILE_LIST
      # 将文件mv到回收站中
      mv $tmp_path $HOME_TRASH_PATH/$trash_name
   done
}

```

## 显示回收站中的文件

```shell
# 显示回收站中垃圾清单
function _trash_list()
{
   # 为输出加上头
   echo -e "==== Garbage Lists in $HOME_TRASH_PATH ===="
   _trash_help
   # 列表文件
   ls -a $HOME_TRASH_PATH
}

```

## 显示回收站中的文件及路径

```shell
function _trash_path()
{
   # TRASH_FILE_LIST中的文件结构如下
   # c.c.20180531-20:05:57            /home/aaa/test
   # a.b.20180531-20:06:54            /home/aaa/test/file_path
   # b.c.20180531-20:07:32            /home/aaa/test/file_path
   # b.a.20180531-20:10:51            /home/aaa/test/./b.b
   # 头
   echo -e "------------bakup name---------------recover path"
   # 输出文件及路径
   cat $TRASH_FILE_LIST
}
```

## 恢复文件

```shell
# 找回回收站相应文件
function _trash_recover()
{
   local rfile_name=	# 需要恢复的文件名
   local file_name=		# 原始文件名
   local file_path=		# 原始文件路径
   # 同时支持多个文件的恢复
   for tmp_path in $@;
   do
      # 首先判断文件（夹）的存在性，以防mv时出错
      if [[ ! -d "$HOME_TRASH_PATH/$tmp_path" ]] && [[ ! -f "$HOME_TRASH_PATH/$tmp_path" ]]; then
         echo "no such file : $tmp_path"
         continue
      fi
      rfile_name=$tmp_path
      # 匹配去除（.时间）还原真实文件名
      file_name=${rfile_name%\.*}
      # 通过cat/grep/awk获取TRASH_FILE_LIST中文件的原始路径
      file_path=`cat $TRASH_FILE_LIST | grep "$rfile_name" | awk '{print $2}'`
      # 删除该记录，todo
      # 将文件搬移回原始目录中
      mv  $HOME_TRASH_PATH/$rfile_name $file_path/$file_name
   done
}
```

## 清空回收站

```shell
function _trash_clean()
{
   # 以醒目的字体要求再次确认，以免误删
   echo -ne "\033[33mClear all trashes in $HOME_TRASH_PATH, Sure?[y/n]\033[0m"
   read confirm
   if [ $confirm == "y" -o $confirm == "Y" ] ;then
      # 调用系统rm删除HOME_TRASH_PATH下的所有文件
      /bin/rm -rf $HOME_TRASH_PATH/*
      /bin/rm -rf $HOME_TRASH_PATH/.* 2>/dev/null
   fi
}
```

## 命令重构

```shell
trash()
{
   # 指定对不同参数的解析
   local arg=$1
   shift
   case "$arg" in
      -d)
         _trash_delete $@
         ;;
      -l)
         _trash_list
         ;;
      -r)
         _trash_recover $@
         ;;
      -c)
         _trash_clean
         ;;
      -p)
         _trash_path
         ;;
      -h|*)
         _trash_help
         ;;
   esac

}

alias rm="trash -d"		# 重构rm

# 以下内容修改使用，视具体环境
alias rl="trash -l"		# 这些命令基本在系统中并未用到，所以才如此使用
alias rr="trash -r"
alias rp="trash -p"
alias rc="trash -c"
alias rh="trash -h"
```



## 使用说明

```shell
function _trash_help()
{
   echo -e "\033[33m----Usage------\033[0m"
   echo -e "\033[33m-1- Use "trash -p" or "rp" to list all garbages and real pathes in $HOME_TRASH_PATH\033[0m"
   echo -e "\033[33m-1- Use "trash -l" or "rl" to list all garbages in $HOME_TRASH_PATH\033[0m"
   echo -e "\033[33m-1- Use "trash -c" or "rc" to clear all garbages in $HOME_TRASH_PATH\033[0m"
   echo -e "\033[33m-2- Use "trash -d" or "rm" to mv the file into $HOME_TRASH_PATH\033[0m"
   echo -e "\033[33m-2- Use "trash -r" to "rr" to mv the file in garbages to current dir\033[0m"
   echo -e "\033[33m-2- Use "trash -h" to "rh" to show help \033[0m"
}
```

## 自动补全

```shell
function _cmp_rr()
{
   local cur="${COMP_WORDS[COMP_CWORD]}"
   local options=`ls $HOME_TRASH_PATH`
   COMPREPLY=( $(compgen -W "${options}" -- ${cur}) )
}
# 自行查阅complete的用法 complete -F FUNC COMMAND
# -F指定了后面的函数执行结果作为COMMAND的补全内容
complete -F _cmp_rr rr
```

## 自启动

```shell
# 将文件放入～目录下，命名为（.bash_trash），并在~/.bashrc中添加如下代码即可
if [ -f ~/.bash_trash ]; then
  . ~/.bash_trash
fi
```



# 基本测试

```shell
# 命令举例
# 命令连续执行
# pwd = ～/test
# 创建所需文件
$ mkdir a b c
$ touch a/a.a a/a.b a/a.c b/b.a b/b.b b/b.c
$ tree
.
├── a
│   ├── a.a
│   ├── a.b
│   └── a.c
├── b
│   ├── b.a
│   ├── b.b
│   └── b.c
└── c

3 directories, 6 files

# 测试绝对路径
$ rm /home/aaa/test/a/a.a
$ ls a
a.b  a.c

# 测试绝对路径
$ rm ~/test/a/a.b
$ ls a
a.c

# 测试相对路径
$ rm ./a/a.c
$ ls a

# 测试多个文件
$ rm b/b.a b/b.b b/b.c
$ ls b

# 测试通配符
$ rm *
$ ls

# 测试文件列表
$ rl
==== Garbage Lists in /home/aaa/.trash ====
----Usage------
-1- Use trash -p or rp to list all garbages and real pathes in /home/aaa/.trash
-1- Use trash -l or rl to list all garbages in /home/aaa/.trash
-1- Use trash -c or rc to clear all garbages in /home/aaa/.trash
-2- Use trash -d or rm to mv the file into /home/aaa/.trash
-2- Use trash -r to rr to mv the file in garbages to current dir
-2- Use trash -h to rh to show help
.                      a.b.20180531-20:52:24  b.b.20180531-20:53:01
..                     a.c.20180531-20:52:39  b.c.20180531-20:53:01
a.20180531-20:53:16    b.20180531-20:53:16    c.20180531-20:53:16
a.a.20180531-20:52:06  b.a.20180531-20:53:01  .list

# 测试文件路径
$ rp
------------bakup name---------------recover path
a.a.20180531-20:52:06            /home/aaa/test/a
a.b.20180531-20:52:24            /home/aaa/test/a
a.c.20180531-20:52:39            /home/aaa/test/./a
b.a.20180531-20:53:01            /home/aaa/test/b
b.b.20180531-20:53:01            /home/aaa/test/b
b.c.20180531-20:53:01            /home/aaa/test/b
a.20180531-20:53:16              /home/aaa/test/a
b.20180531-20:53:16              /home/aaa/test/b
c.20180531-20:53:16              /home/aaa/test/c

# 测试补全，rr后敲击2次tab
$ rr [tab][tab]
a.20180531-20:53:16    a.c.20180531-20:52:39  b.b.20180531-20:53:01
a.a.20180531-20:52:06  b.20180531-20:53:16    b.c.20180531-20:53:01
a.b.20180531-20:52:24  b.a.20180531-20:53:01  c.20180531-20:53:16

# 测试恢复文件
$ rr a.a.20180531-20:52:06
$ tree
.
└── a
    └── a.a

1 directory, 1 file
```

大功告成。

# 相关链接

[源文件下载](http://p7s40q0ob.bkt.clouddn.com/files/tools/bash_trash	"源文件下载")

[源代码更新请戳此](https://github.com/KingRumn/MyWorkEnv/tree/master/dotfiles "代码库目录")



# 参考资料

