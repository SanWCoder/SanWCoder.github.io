---
title: 使用终端如何上传代码到GitHub
comments: true
date: 2017-04-10 11:30:02
tags:
 - git
 - github
categories:
 - github
photos:
thumbnail:
---

本篇文章简述下如何将新项目通过终端上传到github，并使用git进行版本开发
<!-- more -->


##  配置SSH通道

* 查看是否存在隐藏文件.ssh  如果存在删除文件夹重新生成key

 `/Users/《#用户名#>/.ssh/id_rsa`  
 
* 生成SSHkey要求输入密码啥的一直回车  

`ssh-keygen -t rsa -C "GitHub登录名<#xxxxxxx@163.com#>"`

* 成功以后

```objc
pbcopy < ~/.ssh/id_rsa.pub // 复制生成的key
```
* 到GitHub->setting->SSH and GPG keys 添加粘贴复制的key就算配置完成
* 添加SSH到GitHub

```objc
ssh -T git@github.com  // 执行完这条指令之后会输出  Are you sure you want to continue connecting (yes/no)?  输入 yes 回车
回到github，刷新网页就可以看到钥匙旁的灰色小圆点变绿，就表明已经添加成功了。
```
##  本地创建本地git库

```objc
mkdir LiteraryHeaven<#git文件夹名称#> // 创建git文件夹
cd LiteraryHeaven // 跳转到相应文件夹
git init // 创建git本地库，之后将你的文件放到文件夹内
git status // 查看本地变了的文件，需要添加的问红色，需要提交的为绿色
git add a.tex<#要添加到git本地库的文件名称#> // 或者使用 git add . 添加所有修改文件，
git commit -m"提交日志"
```
##  创建远程git库

* 进入GitHub，创建一个远程库
* 进入新建的远程库以SSH方式查看远程库地址，进行复制

## 关联本地库和远程库

```objc
// 1.关联远程库
git remote add origin git@github.com:xxxx/xxxx.git <#远程库地址t#>  
// 2.要把远程和本地两个不同的项目合并
// 如果直接 pull 会报 fatal: refusing to merge unrelated histories 因此需要添加 --allow-unrelated-histories
git pull origin master --allow-unrelated-histories 
// 3.推送本地分支到远程
git push origin master 
// 如果报错，意思是push的本地库版本是在远程版本之后使用
git remote origin -f // 强制推送
git branch -a // 查看远程和本地所有分支
git pull origin master // 拉取远程库到本地库
git push origin :<#branchName#>[分支名] // 推送本地分支到远程分支
```
## 其他相关git操作请转至[git终端操作语法](/_posts/git终端操作语法)

