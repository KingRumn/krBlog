---
title: hexo 添加标签和分类
date: 2018-04-27 23:00:34
categories:
- hexo
tags:
- linux
- hexo
- ubuntu
---

整个过程**分类** 与 **标签** 极其类似，只需要将对应的`categories`换成了`tags` 即可

# 创建页面

新建一个页面命名为`categories`

```
$ hexo new page categories		#tags
```

# 编辑页面

编辑新建的页面`source/categories/index.md`

```
---
title: 分类
date: 2018-04-25 22:34:08
type: "categories"
---
```

```
---
title: 标签
date: 2018-04-25 22:34:08
type: "tags"
---
```

# 编辑菜单

在菜单中增加链接，编辑主题下的`_config.yml`， 将`menu`中的`categories: /categories`注释去掉：

```
menu:
  home: / || home
  categories: /categories/ || th
  archives: /archives/ || archive
  tags: /tags/ || tags
```

# 实际使用

在文章中添加分类

在文章的头中增加`categories`描述

```
title: hexo 添加标签和分类
date: 2018-04-27 23:00:34
categories:
- hexo
tags:
- linux
```

# 配置别名

在配置中更新别名

其中`:` 以前的是正式的名称，以后的是访问的路径

```
category_map:
        hexo:hexo
        ubuntu:ubuntu
        computer vision: computer-vision
```

```
tag_map:
        linux:linux
        ubuntu:ubuntu
        vim:vim
        tmux:tmux
        hexo:hexo
        yolo:yolo
        opencv:opencv
        computer vision:computer-vision
```

# 部署生效

一定要记得先clean在生成

```
$ hexo clean
$ hexo g
$ hexo d
```



