---
title: 使用 Travis CI 自动部署 Github Pages
date: 2017-04-23 11:32:32
tags: [Hexo, Travis]
---

## 参考

[使用 Travis CI 自动更新 GitHub Pages](http://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/#comments)
[How to publish to Github Pages from Travis CI?](http://stackoverflow.com/questions/23277391/how-to-publish-to-github-pages-from-travis-ci)

## 一切为了自动化

关于 Github Pages 的自动化，我想可以分为三个阶段：农耕时代，半自动化时代，自动化时代。

来解释一下，农耕时代的操作是写好博客后，手工在命令行执行 `hexo g` ，然后手工将 public 目录下的静态文件复制到 Github Pages 项目中，再用 git 进行相应操作并 push 到 github。这一系列的操作虽然不出错，但是流程过于繁琐，特别是对于经常写博客的同学来说。

所谓自动化时代，常见的做法使用 hexo 自带的 `hexo deploy`。在进行相应的配置之后， 每次写好博客，然后 `hexo d` 就可以进行部署了。然后这样依旧不够自动化，每次写好博客之后都需要手工 `hexo g`，所以进入下一个阶段。

而自动化时代，便是全自动化，当写好博客后，并把博客源码 push 到 github 就会触发一系列的部署动作，无需手工部署。具体实操可以使用 Travis CI 或是 Jenkins 这样的自动化部署工具，最后我选择的是前者，原因是免费（逃
<!-- more -->

## 原理

![workflow](http://7xoxnz.com1.z0.glb.clouddn.com/gh-tavis-wf.jpeg)

## 具体流程

1. 在 github 上生成 Personal access token 供 Travis 使用，[详细步骤](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/#creating-a-token)
**注意：** a. 在 Select scopes 时如果你的静态网页仓库是 public 的，需要勾选 `public_repo`；如果是 private 的，则需要勾选 `repo` (Full control of private repositories)。没有正确勾选的话，Travis 进行 push 时会出现 403 错误。wfy，Token 值要妥善保存。

2. 在 Travis 线上配置 github token 值或是 Travis 的 命令行工具本地加密 token。这里我推荐使用前者，原因是无需安装 ruby 和 travis 依赖，如果按照的后者方案的话，可以看看参考中的第一篇，有详细的步骤。
来看看线上配置 Token 的步骤：在 Travis 的仓库设置中(`https://travis-ci.org/<me>/<myrepo>/settings`) 新建一个环境变量：`GH_TOKEN=<token>`，同时确保 "Display value in build log" 是 "Off" 的。这样会确保 token 明文不会再 Travis 日志中出现，一旦别人得到了这个 token 就可以对你的仓库进行 push 操作了。

3. 博客源码根目录下配置自己的在 `.travis.yml` 文件，可以参考我的配置
``` yml
language: node_js
node_js: stable

cache:
  directories:
  - node_modules

# 环境变量
env:
  global:
  - GH_REF: github.com/zerosrat/zerosrat.github.io.git

# 只检测 master 分支的 push
branches:
  only:
  - master

# build lifecycle start
before_install:
- npm i -g hexo-cli
install:
- npm i
script:
- hexo g
after_script:
- cd public
- git init
- git config user.name "zero_ai"
- git config user.email "zero.yu@hotmail.com"
- git add --all
- git commit -m "Update blog via travis"
- git push -f -q "https://${GH_TOKEN}@${GH_REF}" master:master
# build lifecycle end
```

4. 进行一次 push，上 Travis 看执行 log。

---
