---
title: npm 下载提速解决方案
date: 2016-11-19 20:42:49
tags: [npm, yarn]
categories: Front End
---

## 参考

[使用npm安装一些包失败了的看过来](https://cnodejs.org/topic/4f9904f9407edba21468f31e)
[npm配置镜像、设置代理](https://segmentfault.com/a/1190000002589144)
[安装 node-sass 的正确姿势](https://github.com/lmk123/blog/issues/28)

## 前言

最近饱受 npm 下载速度慢之苦，自然有了提速的需求。网上查阅资料后，总结出了三个常用方案：使用淘宝源、`cnpm` 以及最近大热 `yarn`
<!-- more -->

---
## Plan A: 使用淘宝源

### 一次性使用
``` bash
$ npm install express --registry https://registry.npm.taobao.org/
```

### 持续性配置
两种子方案：直接配置和使用 nrm 快速切换 npm 源
- 直接配置
  ``` bash
  // use tabao registry
  $ npm config set registry https://registry.npm.taobao.org/

  // verify
  $ npm config get registry
  ```
  ``` bash
  // restore to default
  $ npm config set https://registry.npmjs.org/
  ```

- 使用 nrm 快速切换 npm 源
  使用 [nrm](https://github.com/Pana/nrm) 可以简单而快速地切换到不同的 registries，包括：npm, cnpm, taobao, nj(nodejitsu), rednpm.
  ``` bash
  // install nrm
  $ npm i nrm -g

  // list available registries
  $ nrm ls

  // use taobao registry
  $ nrm use taobao

  // speed test
  $ nrm test
  ```

### 指定项目的npm registry
在项目根目录下新建 .npmrc 文件，其内容为：`registry=https://registry.npm.taobao.org
`

---

## Plan B: ~~cnpm~~
> [cnpm](https://github.com/cnpm/cnpm): npm client for China mirror of npm https://npm.taobao.org


``` bash
// install cnpm
$ npm install cnpm -g --registry=https://registry.npm.taobao.org
```
之后可以通过 cnpm 命令来安装包了，其命令和 npm 大同小异，如
`$ cnpm i webpack -g`

> **warning**: cnpm有个致命缺陷，用它下载安装的模块都是以软链形式存在的，本来我们的模块文件就多，再加个软链又多一倍文件，导致有些编辑器（sublime text）和 IDE（WebStorm）检索目录时非常慢，甚至卡死

---
## Plan C: yarn
> [yarn官网](https://yarnpkg.com)

yarn 是 facebook 发布的一个全新的包管理工具，出现的缘由是为了解决 npm 遗留下的一些痛点。yarn 优点是快速，安全，可靠

### 安装
[根据自己的操作系统进行安装](https://yarnpkg.com/en/docs/install)
安装完成后测试：`$ yarn --version`

### 常用命令
- `$ yarn add`: adds a package to use in your current package.
- `$ yarn init`: initializes the development of a package.
- `$ yarn install`: installs all the dependencies defined in a package.json - file.
- `$ yarn publish`: publishes a package to a package manager.
- `$ yarn remove`: removes an unused package from your current package.

详情参考[官方文档](https://yarnpkg.com/en/docs/cli/)

---
## Misc

安装 node-sass 时在 node scripts/install 阶段会从 github.com 上下载一个 .node 文件，大部分安装不成功的原因都源自这里，因为 GitHub Releases 里的文件都托管在 s3.amazonaws.com 上面。Thanks to GFW，这个网址在国内总是网络不稳定，所以我们需要通过第三方服务器下载这个文件。

解决方法是在项目内添加一个 .npmrc 文件，文件的内容是设置了一些难易安装成功的包的第三方地址（最后一行可以按需去掉）

```
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl=https://npm.taobao.org/mirrors/phantomjs/
electron_mirror=https://npm.taobao.org/mirrors/electron/
registry=https://registry.npm.taobao.org
```
---
## 总结

推荐使用方案一 (**npm + taobao registry**) 和方法三 (**yarn**)。

## 延伸阅读

[如何评价Facebook推出的JavaScript模块管理器yarn？](https://www.zhihu.com/question/51502849)
