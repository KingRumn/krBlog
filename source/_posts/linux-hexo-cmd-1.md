---
title: hexo 基础命令
categories:
- hexo
---

# 背景
各种各样的博客五花八门，但是利用`github`构建个人看起来很有逼格，在其他地方积累的并不多，因此花了2天的时间研究了一下如何依靠github构建个人博客。
- 主流的个人博客工具包括`hexo`，`jekyll`，`wordpress`等，具体的优劣建议参考：`https://www.jianshu.com/p/c4f145fdd637`
- 个人最终选择了`git + github + hexo`的方案，主要是命令少，易于学习。

# 安装git
```
# 安装
$ sudo apt-get install git
# 配置账户
$git config --global user.name "your username" 
$git config --global user.email "your_email@youremail.com"
$ssh-keygen -t rsa -C "youremail@email.com"
```

# github pages
1. 注册账户
如果你还没有`github`的账号，去注册一个吧（`https://github.com/join?source=header-home`），一个有逼格的程序员，这是必须的

2. 创建github pages
- 登录（`https://github.com`）
- 找到`Start a project`或者`New repository`创建个项目
```
yourname/yourname.github.io	
# 这个地址必须严格按这个写，错了是无法访问的
```
**Notice**: `yourname`必须是你注册时的用户名，如果你忘记了，去点下你的头像，下拉出来的`Signed in as `后面就是你的名字

3. 书写第一个主页
- pull代码
```
$ cd yourworkdir 	#yourworkdir是工作目录
$ git clone https://github.com/yourname/yourname.github.io
$ cd yourname.github.io
$ touch index.htm	# 创建默认页面
```
- 创建页面
通过vim或gedit编辑`index.htm`并输入以下内容
```
<html>

<head>
<title>My Blog</title>
</head>

<body>
<p>Hello，world！</p>
<p>This is my first blog. </p>
</body>

</html>
```
- 上传代码
```
$ git add .
$ git commit -m "创建我的第一个页面"
$ git push origin master
```
后续一般需要输入用户名（yourname）和密码（登录密码）

4. 访问页面
创建成功后，通过`https://yourname.github.io`即可访问刚才书写的页面

这样你的个人博客已经可以访问了，如果你已经可以自己构建个网站结构，那就直接开搞吧，不过作为看这个教程的人，一般都需要去寻找一个现成的搭建博客的方案，比如`hexo`,`jekyll`等，下面先介绍`hexo`的使用过程。

# 安装hexo
1.  安装nodejs
不建议使用apt-get方式安装，容易失败；
建议使用nvm工具进行安装（nvm是一个可以让你在同一台机器上安装和切换不同版本node的工具）
使用如下命令进行安装：
```
export NVM_DIR="$HOME/.nvm" && (
  git clone https://github.com/creationix/nvm.git "$NVM_DIR"
  cd "$NVM_DIR"
  git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" origin`
) && . "$NVM_DIR/nvm.sh"
```
使nvm开机启动，将如下内容添加到`~/.bashrc`中：
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
```
重启 `terminal `或者重新` source `一下 `~/.bashrc`；
执行以下命令：
```
$nvm ls-remote //列出所有安装包的版本信息
$nvm install stable //安装最稳定版本
$nvm use node //使用当前版本
$nvm run node --version //也可以使用该命令 使用当前版本
$nvm alias default node //设置默认版本
```
如果需要更新则手动更新 nvm:
```
(
 cd "$NVM_DIR"
 git fetch origin
 git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" origin`
) && . "$NVM_DIR/nvm.sh"
```
2. 安装hexo
```
$npm install  hexo-cli -g
$npm install hexo-server -g
$npm install hexo-deployer-git -g
$npm install hexo-util -g
```
接下来建立博客目录，并初始化：
```
$ cd yourworkdir
$ mkdir yourblogdir
$ cd yourblogdir
$ hexo init //初始化 下载必要的建站文件
$ npm install //安装依赖项
```

接下来就可以开始博客的撰写了。
# 配置hexo
1. 选择主题
2. 修改markdown渲染引擎

# 撰写博客

Todo

# 发布博客

Todo

# 参考
[ubuntu 下 hexo 安装与配置](http://stevenshi.me/2017/05/23/ubuntu-hexo/)
[jekyll](http://localhost:4000/2018/04/25/linux-hexo-guide/)
[hexo](http://localhost:4000/2018/04/25/linux-hexo-guide/)
[pandoc](http://localhost:4000/2018/04/25/linux-hexo-guide/)


























