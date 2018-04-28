---
title: git终端操作语法
comments: true
date: 2017-07-11 11:12:29
tags:
 - git
categories:
 - git
photos:
thumbnail:
---

git是一种分子式版本管理工具，git的操作有许多可视化工具如sourceTree等，但这篇文章主要写下如何使用git主评断语法来操作git

<!-- more -->

## 代码提交规范

```objc
eg:  
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>g
<footer>

```
### **type** 用于说明 commit 的类别，只允许使用下面7个标识。  
* *docs*: 仅文档更改
* *feat*: 一个新功能
* *fix*: 修复错误
* *perf*: 改进性能的代码更改
* *refactor*: 代码更改，既不修复错误也不添加功能
* *style*: 不影响代码含义的变化（空白，格式化，缺少分号等）
* *test*: 添加缺失测试或更正现有测试
* *chore*: 改变构建流程、或者增加依赖库、工具等  

### **scope** 用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。
### **subject**是 commit 目的的简短描述，不超过50个字符。

```objc
eg：
feat: all middleware support async function and common function
feat: all middleware support async function and common function
docs: add quickstart.md
```
## Git终端基本操作

### 操作流程

```objc
// 创建本地仓库并和远程仓库关联
// 1.创建本地存放git的文件目录
mkdir <#gitTest#>[目录名]
// 2.进入到当前目录
cd gitTest
// 3.创建一个本地文件并编辑
touch a.txt
open a.txt
// 4.创建本地git仓库
git init
// 5.查看本地仓库文件变化（变红）
git status
// 6.将本地项目工作区的所有文件添加到暂存区
git add .(第一次提交所有文件到本地暂存区)
git add a.txt （有变化的文件添加到暂存区）
// 6.再次查看本地仓库文件变化（变绿）
git status
// 7.将暂存区文件提交到本地仓库
git commit -m "测试"
// 8.再次查看本地仓库文件变化（显示没有变化）
git status
// 10.然后在相应的GitHub/GitLab上创建远程仓库
// 11.创建完成以后配置相关用户信息
git config --global user.name "wenweiwei"
git config --global user.email "wenweiwei@16lao.com"
// 12.将本地仓库与远程仓库关联
git remote add origin <#git@192.168.3.191:wenweiwei/GitTestProject.git#>[远程仓库地址]
// 12.1 修改关联远程库
git remote set-url origin <#git@118.89.225.146:16lao/ios.git>[新远程仓库地址]
// 12.2 删除当前关联的远程库
git remote rm origin 
// 13.更新远程库到本地
git pull origin master  
// 14.将本地仓库文件推送到远程仓库
git push origin master
// 15.查看远程仓库
git remote -v

```

### 基本操作

```objc
// 1. 查看目前代码的修改状态:
git status
// 2. 查看文件修改内容
git diff  <#a.txt#>[文件名]
// 3.删除文件
git rm <#a.txt#>[文件名]
// 4.添加修改文件到暂存文件
git add <#a.txt#>
git add <#b.txt#>
// 5.提交已暂存的文件到本地仓库
git commit -m "添加文件"
// 6.同步远程仓库到本地仓库
git pull origin master
// 7.有冲突解决冲突后推送，没有直接推送到远程仓库
git push origin master
// 8.查看当前分支
git branch (绿色标志为当前分支)
git branch -a (查看远程分支，远程分支用红色表示，当前本地分支用绿色表示)
// 9.创建分支
git branch <#branchName#>[分支名]
// 10.检出分支
git checkout <#branchName#>[分支名]
// 11.创建并切换到新分支
git checkout -b <#branchName#>[分支名]
// 12.合并分支
git merge <#branchName#>[要合并到当前分支上的分支名]
// 13.删除分支
git branch -d <#branchName#>[分支名]
git push --delete origin<#branchName#>[分支名](删除远程分支)
// 如何切换远程分支 1.现在本地创建分支 2.更新远程分支到新建的本地分支
// 14.推送远程分支
git push origin :<#branchName#>[分支名]
// 15.查看标签
git tag
// 16.创建标签
git tag v1.0.0
// 17.查看日志
git log（提交Id，提交人，提交日期，提交msg）
git log --pretty=oneline --abbrev-commit （提交简短Id，提交msg）
// 18 .补打标签
git tag v1.1.1 <#6224937#>[提交Id]
// 19.查看标签信息
git show <#tagName#>[标签名]
// 20.删除标签
git tag -d <#tagName#>[标签名]
// 21.推送标签到远程
git push origin <#tagName#>[标签名]
// 22.推送所有本地标签到远程
git push origin --tags
// 23.删除远程标签
git tag -d <#tagName#>[标签名] // 删除本地标签
git push origin :refs/tags/<#tagName#>[标签名] // 删除远程标签
// 24.查看远程tag
git fetch origin tag <#tagName#>[标签名]
// 25.克隆远程仓库(先进入到需要克隆岛本地目录)
git clone <#git@192.168.3.191:wenweiwei/GitTestProject.git#>[远程仓库地址]
```

