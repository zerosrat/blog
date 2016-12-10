---
title: Hexo + Github Pages 博客的搭建与迁移
date: 2016-11-13 19:30:51
tags: [Blog, Hexo]
categories: Blog
---

## 参考

> [手把手教你使用Hexo + Github Pages搭建个人独立博客](http://jiji262.github.io/2016/04/15/2016-04-15-hexo-github-pages-blog/)

## 开始之前

到 [Nodejs 官网](https://nodejs.org/en/download/) 下载适合自己操作系统的安装文件，完成后在终端输入 `$ npm -v` 进行验证

到 [Git 官网](https://git-scm.com/download/) 下载适合自己操作系统的安装文件，完成后再终端输入 `$ git --verion` 进行验证
<!-- more -->
## 初识 Hexo

1. 安装 Hexo
``` bash
$ npm install -g hexo-cli
```

2. 进行博客初始化
到自己希望放置博客的地方创建 blog 文件夹，并在终端进入到该目录下
``` bash
$ hexo init
$ npm install
```

3. 本地开启服务器，看看自动生成的第一篇博客吧
``` bash
$ hexo server
```
  打开 http://localhost:4000/ 看看吧，是不是有点小激动呢。别急，还没有部署到 Github Pages 上呢。

4. 最常用的 Hexo 命令
```bash
$ hexo new  // 生成一篇新博客到 ./source/_post 目录下
$ hexo generate  // 生成待部署的文件到 ./public 目录下
$ hexo clean  // 清除待部署的文件
$ hexo server  // 开启本地服务器
```

## 初识 Github Pages

开始前我们需要自己的一个 [github](https://github.com) 账号，没有的话就注册一个吧

接下来，可以[参考官方的指南](https://pages.github.com/)，进行如下操作：
1. 创建一个仓库，名字必须是 username.github.io（注：username是自己 github 的账户名，如我的：zerosrat.github.io）

2. 到自己希望放置博客的地方，把仓库克隆下来（建议和 Hexo 位于同一目录下）
``` bash
git clone https://github.com/username/username.github.io
```

3. 推到云端
把 Hexo 目录下 public 文件夹的全部内容复制到 username.github.io 文件夹下
``` bash
git add *
git commit -m 'init'
git push
```

4. 查看效果
在浏览器输入自己博客的 url 如 http://username.github.io ，到这里是不是发现自己的本地内容和云端同步了呢 :)


## 备份自己博客的源文件（可选）

虽然说这一步是可选的，但是这步对于博客的备份与迁移极为重要。

1. 在 Github 上创建名为 blog 的仓库

2. 回到本地，进行同步（username 需要替换哦）
``` bash
$ cd blog
$ git init
$ git add *
$ git commit -m 'init'
$ git remote add origin https://github.com/username/blog
$ git push
```

## 一个简单的流程

- 举个栗子，故事的主人公小明有一天想写博客了
- 首先他到 Hexo 项目路径新建了一篇博客：`$ hexo n '我的博客'`；
- 然后进入 ./source/_post 目录下找到 `我的博客.md` 文件，打开进行创作；
- 创作的同时打开本地服务器：`hexo s`，可以随时查看效果；
- 经历了一下午的时间， 小明终于创作完成了；
- 这时候应该生成本地网页文件了：`hexo clean` + `hexo g`，然后手动将 ./public 目录下的文件拷贝到 xiaoming.github.io 目录下进行覆盖；
- 最后把文件部署到 github 上： 可参考上一章的第四步。

## 博客的迁移

- 有一天，故事的主人翁小明换电脑了，他突然发现自己的博客内容还在原来的电脑上，没有备份，小明同学很着急
- 没有关系，小明同学之前将 blog 项目和 xiaoming.github.io 项目同步到了 Github 上，这时小明只需要再把它们克隆下来
- 进行克隆：
``` bash
$ git clone https://github.com/xiaoming/blog
$ git clone https://github.com/xiaoming/xiaoming.github.io
```

- 本地博客初始化：
``` bash
$ cd blog
$ npm install
$ hexo s
```

- 再次访问 http://localhost:4000 ,小明发现自己的博客又回来啦！

---
EOF
