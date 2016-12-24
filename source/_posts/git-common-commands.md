title: Git 常用命令
date: 2016-04-17 17:56:59
tags: Git
categories: Git
---

## 前言

谈到协同开发，就不得不提到Git了。熟练使用Git命令对开发的帮助很大。这里我将命令分为了两个部分--基础类和分支类。
<!-- more -->
***

## 常用命令

### 基础类

- 在工作目录初始化
`$ git init`

- 克隆远端仓库，第二个参数可选
`$ git clone [path] [localName]`

- 跟踪新的文件
`$ git add`

- 检查仓库中文件的状态，添加'-s'的话显示会更加简洁
`$ git status (-s)`

- 从跟踪清单移除文件
`$ git rm --cached [file-name]`

- 提交之前进行检查
`$ git diff --check`

- 提交
`$ git commit (-a) (-m) [comments]`
eg: `$ git commit -a -m 'added new benchmarks'`

- 修改最后一次提交
```
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

- 丢弃所有未暂存的修改
`$git checkout .`

- 查看当前的远程库信息
`$ git remote (-v)`
`$ git remote show [remote-name]`

- 添加远程仓库
`git remote add [shortname] [url]`

- 从远程仓库抓取数据
`$ git fetch [remote-name]`

- 推送数据到远程仓库
`$ git push [remote-name] [branch-name]`

- 远程仓库删除
`$ git remote rm [remote-name]`

### 分支类

- 创建分支
`$ git branch [branch-name]`

- 切换分支
`$ git checkout [branch-name]`

- 创键并切换分支
`$ git checkout -b [branch-name]`

- 合并分支
`$ git merge (--no-ff) [source-branch-name]`

- 远程抓取数据并合并
`$ git pull`
或
```
$ git fetch [remote-name]
$ git merge (--no-ff) [remote-name]
```

- 删除分支
`$ git branch -d [branch-name]`

- 查看分支信息
`$ git branch (-v)`
`$ git branch --merged`
`$ git branch --no--merged`

- 跟踪远程分支
`$ git checkout -b [branch] [remote]/[branch]`
`$ git checkout --track [remote]/[branch]`

- 删除远程分支
`$ git push [remote]:[branch]`

- 衍合
```
$ git checkout dev
$ git rebase master
```

- 分支指针回滚到某个commit
`$git reset --hard [commit-code]`
