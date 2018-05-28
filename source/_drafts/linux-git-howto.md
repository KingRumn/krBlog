---
title: git 笔记
date: 2018-05-28 20:37
categories:
- linux
tags:
- linux
- git
---



# 认证

此处为了解决向`github`提交代码需要不断输入用户名密码的问题。

## 方案一 通过保存历史记录的方式

1. 在命令行下执行如下命令：

```shell
git config --global credentialhelper store
```

这一步会在用户目录下的`.gitconfig`文件末尾添加如下字段，表示会记录用户名和密码

```shll
[credential]
	helper = store
```

2. 现在`push`代码，此时会要求你输入用户名和密码，这一步的输入会被记住，下次push时就不用输入用户名密码了。此时会生成`.git-dredential`记录用于记录用户名和密码信息（明文，需慎重）；

## 方案二 将https改为ssh方式

--ToDo