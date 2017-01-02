---
title: npm 常用命令
date: 2016-07-15 15:26:03
tags: npm
categories: Front End
---

## 参考

> [npm 官方文档](docs.npmjs.com)

***

## 开始之前

### 什么是 npm

npm 全称是Node Package Manager，可以类比 ruby 的 gem 或是 java 的 maven
<!-- more -->

### 环境搭建

到 Nodejs 官网下载并安装 Nodejs 即可。之后命令行输入 `$ npm -v` 验证。

## 常用命令

- 当前目录生成 package.json
`npm init` 手动配置
`npm init -y` 默认配置

- 安装 npm 包
`npm install npm-package` 局部安装 npm 包
`npm install -g npm-package` 全局安装 npm 包
`npm install --save npm-package` 局部安装 npm 包，并在 package.json 中更新 dependencies 的配置
`npm install --save-dev npm-package` 局部安装 npm 包，并在 package.json 中更新 devDependencies 的配置

- 更新 npm 包
`npm outdate` 查看项目中非最新版本的 npm 包
`npm update` 更新项目中 npm 包
`npm outdate -g --depth=0` 查看全局非最新版本的 npm 包
`npm update npm-package` 更新全局中某 npm 包

- 卸载 npm 包
`npm uninstall npm-package` 局部卸载 npm 包
`npm uninstall -g npm-package` 全局卸载 npm 包
`npm uninstall --save npm-package` 局部卸载 npm 包，并在 package.json 中更新 dependencies 的配置
`npm uninstall --save-dev npm-package` 局部卸载 npm 包，并在 package.json 中更新 devDependencies 的配置

- 列出 npm 包
`npm ls` 列出当前目录环境下的 npm 包
`npm ls -g --depth=0` 列出全局 npm 包

- 添加 npm 账户
`npm adduser`

- 发布自己的 npm 包
`npm publish`

- 列出配置信息
`npm config ls`

***
EOF
