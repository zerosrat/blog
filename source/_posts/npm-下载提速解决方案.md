---
title: npm 下载提速解决方案
date: 2016-11-19 20:42:49
tags: npm
categories: Front End
---

## 参考

[使用npm安装一些包失败了的看过来](https://cnodejs.org/topic/4f9904f9407edba21468f31e)
[npm配置镜像、设置代理](npm配置镜像、设置代理)

## 前言

最近饱受 npm 下载速度慢之苦，自然有了提速的需求。网上查阅资料后，总结出了三个常用方案：使用淘宝源、`cnpm` 以及最近大热 `yarn`

## 使用淘宝源

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

## cnpm
[cnpm](https://github.com/cnpm/cnpm): npm client for China mirror of npm https://npm.taobao.org
``` bash
// install cnpm
$ npm install cnpm -g --registry=https://registry.npm.taobao.org
```
之后可以通过 cnpm 命令来安装包了，如
`$ cnpm i webpack -g`

## yarn
挖坑

## 总结

推荐使用方案一，也是我目前使用的方案，简单快捷

## 延伸阅读

[如何评价Facebook推出的JavaScript模块管理器yarn？](https://www.zhihu.com/question/51502849)
[是时候放弃用 cnpm 命令了](https://cnodejs.org/topic/552212ba01b6c9310d8e9959)
