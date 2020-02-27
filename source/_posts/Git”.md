---
title: ”Git搭建博客”
tags: 学习心得
categories: hexo
---

####                     搞了一天的git博客 说一下心得

**因为是在win10下搭建的环境  所以需要准备好nodejs 和gitbash**

**编写环境是vscode 。**

**1，在搭建过程中需要改镜像到国内的淘宝**

```
npm install -g  cnpm  --registry=https : //registry.npm.taobao .org 
```

**2，然后在全局安装hexo**

```
cnpm install - g hexo`
```

**3，在本地初始化博客，就是使用hexo初始化一个默认的样式**

```
hexo init blog
```

**这里会在根目录下生成 blog 文件夹 在安装的过程中可以看到hexo在github上复制了一些文件在本地。**

**4，我们要先**

```
cd blog
```

**然后运行** 

```
npm install
```

**5，然后个人博客就搭建成功啦**

```
hexo s
```

 **运行一下  进入localhost：4000查看**

**6，把静态网站推送到github上面**

**首先要先关联远程github仓库,新建仓库时名字要和自己的用户名保持一致**

**下面是我的新建仓库的名字 我的github用户名为wangyiming-666.github.io**

```
wangyiming-666.github.io
```

**7，创建成功后，进入blog目录 执行**

```
npm insatll hexo --save
```

**然后执行 用来生成静态页面**

```
hexo g
```

**8,  可以看到这个过程中hexo生成了许多css，js，html文件，这些文件放在blog的public目录下面，这里我们需要一个插件用来自动部署git的静态文件**

```
cnpm insatll hexo-deployer-git --save
```

**9,然后我们来修改配置项 进入blog文件夹，修改这个目录下面的_config.yml 命令如下：**

```
delopy:
    type: git
    repository: https://github.com/wangyiming-666/wangyiming-666.github.io.git  //这里是你自己的github仓库地址 后面记得加git
    branch: master
```

**10, 然后我们就可以上传我们自己的博客啦！执行命令  另外我们也可以看到自己的github更新了文件信息哦！**

```
hexo d
```

​       **完成后在浏览器输入 就可以看到你自己的网站啦！**

```
wangyiming-666.github.io
```

