---
title: vim复制内容到剪贴板
date: 2018-04-27 04:00:34
categories:
- ubuntu
tags:
- linux
- ubuntu
- vim
---

# 问题背景

VIM使用过程中需要将内容直接复制到系统剪贴板，使用场景：

1. 在vim编辑过程中，需要将某些内容直接复制，并粘贴到博客或gedit中；
2. 在2个窗口中分别用vim编辑，此时无法通过vim操作完成，只能借助系统剪贴板



# 解决方案

## 判断支持性

首先需要检查vim是否支持系统剪贴板

```
$ vim --version | grep "clipboard"
> -clipboard
```

如果`clipboard`前面有个`-` ，则说明vim并不支持系统剪贴板。

## 启用vim对剪贴板的支持

- 方案一：安装图形化界面vim

```
$ sudo apt-get install vim-gnome
```

- 方案二：重新编译vim，并开启支持

此时，重新检查，判断`-` 是否变成了`+`，如果是`+`说明已经支持了。

## vim寄存器

打开vim，输入`:reg`查看vim寄存器，当支持clipboard之后，会多出`"+`寄存器，表示系统剪切板，在vim中进入visual视图后使用`"Ny`(N表示特定寄存器编好)，将内容复制到特定的剪切板，那么我们的目的是要复制到系统剪切板则需要选中内容后输入命令：

```
"+y		#该命令将内容复制到系统剪贴板
```

## 设置快捷键

通过`vim ~/.vimrc`, 编辑快捷键，一般在visual模式下才会复制，所以只需要进行vmap就可以了。有人喜欢用`ctrl+c/v`进行复制粘贴，但是与原有的ctrl+v冲突，故而采用leader+c/v的形式进行。

```
let mapleader="," 

"复制粘贴
vmap <leader>v "+gp  "粘贴并使光标移动到内容之后
vmap <leader>c "+y   "复制到系统剪贴板
```

好了，问题解决，可以正常进行复制粘贴了。
