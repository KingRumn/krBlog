---
title: hexo 简介
date: 2018-04-27 22:00:34
categories:
- hexo
tags:
- hexo
- linux
- ubuntu
---

# 基本介绍

Hexo是一个快速/简洁/高效的博客框架，使用Markdown作为渲染引擎解析文章，结合丰富的主题，分分钟生成绚丽的静态网页，是博客写作的首选。



```
.
├── _config.yml		# 站点配置文件
├── node_modules	# 中间产物
├── package.json	# 应用程序信息，包括markdown渲染器等
├── public			# hexo生成的页面信息
│   ├── 2018
│   ├── archives
│   ├── categories
│   ├── css
│   ├── images
│   ├── index.html
│   ├── js
│   └── lib
├── README.md		# 介绍文件
├── scaffolds		# 模板文件，新建文章时，Hexo会据此来建立新的文件
│   ├── draft.md	# 草稿
│   ├── page.md		# 
│   └── post.md		# 文章
├── source
│   ├── categories	# 资源文件，除_post外，开头命名为_的文件/文件夹和隐藏文件在生成过程中会被忽略
│   ├── _drafts		# 草稿
│   └── _posts
└── themes			# 主题文件夹
    ├── hexo-theme-next	# next主题
    └── landscape		# 默认主题
```



# 基本命令

## init

新建一个网站，如果folder可选，在当前目录中建立网站。

```
$ hexo init [folder]
```

## new

新建一篇文章，layout可选，默认使用default_layout替代参数，如果标题包含空格，需要用引号括起来。

会在source下对应的文件夹生成相应的文件。

```
$ hexo new [layout] <title>
```

## generate

生成静态页面。

```
$ hexo generate		# 简写为 hexo g
```

| 选项         | 描述               |
| ------------ | ------------------ |
| -d, --deploy | 文件生成后立即部署 |
| -w, --watch  | 监视文件变化       |

## public

发表草稿。

```
$ hexo public [layout] <filename>
```

## server

```
$ hexo server		# 简写为 hexo s
```



启动服务器，默认情况下，网址为：`http://localhost:4000`

| 选项         | 描述                       |
| ------------ | -------------------------- |
| -p, --port   | 重设端口                   |
| -s, --static | 只使用静态文件             |
| -l, --log    | 启动日志记录，使用覆盖格式 |

## deploy

部署网站

```
$ hexo deploy		# 简写为 hexo d
```

| 选项           | 描述                     |
| -------------- | ------------------------ |
| -g, --generate | 部署之前预先生成静态文件 |

## render

渲染文件

```
$ hexo render <file1> [file2]
```

| 选项         | 描述         |
| ------------ | ------------ |
| -o, --output | 设置输出路径 |

## migrage

从其他博客系统迁移内容

```
$ hexo migrate <type>
```

## clean

清除缓存文件(db.json)和已生成的静态文件(public)，更换主题后，这是必须的。

```
$ hexo clean
```

## list

列出网站资料

```
$ hexo list
```

## version

显示hexo版本

```
$ hexo version
```

## 选项

```
$ hexo --safe		#安全模式，不载入插件和脚本，可用于插件安装遭遇问题时
$ hexo --debug		#调试模式，在终端显示调试信息，并记录到debug.log
$ hexo --silent		# 简洁模式，隐藏终端信息
$ hexo --config custom.yml		# 自定义配置文件路径
$ hexo --draft			# 显示source/_draft文件夹下的草稿文件
$ hexo --cwd /path/to/cwd	#自定义当前工作目录的路径
```

