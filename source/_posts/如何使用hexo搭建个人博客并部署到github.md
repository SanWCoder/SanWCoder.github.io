---
title: 如何使用hexo搭建个人博客并部署到github
comments: true
date: 2018-04-04 11:47:54
tags:
 - github
 - blog
categories:
 - blog
photos:
 - /gallery/blog.jpeg
thumbnail:
 - /gallery/blog.jpeg
---

使用Hexo搭建个人博客

<!-- more -->

## 安装&搭建

#### 安装Node.js
+ 到[node.js官网](https://nodejs.org/en/)下载相应版本按照提示安装
+ 使用Homebrow安装

```objc
// 1. 安装HomeBrow
~ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
// 2. 安装node
~ brew install node
// 3. 安装完成后测试是否安装成功
// 3.1 查看node版本
~ node -version
// 3.2 查看npm版本
~ npm -version
```
#### 安装[Hexo](https://hexo.io/zh-cn/)

```objc
~ npm install -g hexo-cli
```
#### 安装git
```objc
~ brew install git
```
#### 配置GitHub
在你的Github下创建新仓库并将仓库名命名为 **GitHub名.github.io**这样命名后GitHub会自动 启用 GitHub Pages并将你的博客地址设置为 **https://GitHub名.github.io/**
## 初步搭建&本地测试
+ 使用Hexo创建博客

```objc
~ hexo init <folder>
~ cd <folder>
~ npm install

```
+ 创建完成后在相应的文件=夹中会有如下显示

```objc
.
├── _config.yml  // 必要的配置文件，如博客名
├── package.json // 应用程序的信息
├── scaffolds  // 模版文件夹,post.md,drafts.md
├── source    // 资源文件夹
|   ├── _drafts
|   └── _posts
└── themes // 主题文件夹

```
+ 本地测试

```objc
// 清除缓存文件
~ hexo clean 
// 生成静态文件
~ hexo generate/g
// 启动服务器
~ hexo server
// 服务器启动以后再浏览器中打开 http://localhost:4000/即可查看博客
```
## 自定义（以[Icarus](https://github.com/ppoffice/hexo-theme-icarus.git)主题为例）
+ 下载相应的主题[主题](https://hexo.io/themes/)

```objc
~ git clone https://github.com/ppoffice/hexo-theme-icarus.git
```
+ 配置_config.yml

```objc
title: SanW // 标题
subtitle:  // 福标题
description: // 描述
keywords: ios // 搜索关键字
author: // 作者
language: zh-CN // Hexo支持国际化，设置相应语言在/themes/icarus/languages 下
url: http://sanwcoder.github.io // 设置成你的博客地址
// 文件root，如果直接在当前文件夹下设置为/，切记要设置正确，设置错误会加载不出来样式
root: / 
permalink: :year/:month/:day/:title/
permalink_defaults:
// 设置你下载的主题，将你下载的主题放到themes文件夹下，并在这儿设置为相应的主题
theme: icarus
// 部署到github
deploy:
  type: git // 类型为git
  repo: // 仓库的完整地址，在部署时会部署到git库的master分支上

```
+ 在/themes/icarus/_config.yml位置下更改相应主题的_config.yml
 
```objc
// 菜单栏
menu:
  Home: .
  Archives: archives
  Categories: categories
  Tags: tags
  About: about

# Customize
customize:
    // 网站图标
    logo:
        enabled: true
        width: 40
        height: 40
        url: images/icon.jpg
    profile:
        enabled: true 
        avatar: css/images/icon.jpg
        author: // 网站作者
        author_title: // 作者简介
        location: // 位置 FengTai, China
        follow: // 作者github地址
    social_links: // 其他社交链接
        github: 
        Weibo: 
        email: 
    social_link_tooltip: true 
```
+ 部署到github

```objc
// 清除缓存文件
~ hexo clean 
// 生成文件
~ hexo g
// 部署到GitHub
~ hexo d
```
+ 访问https://GitHub名.github.io  

## 添加搜索
+ 在/themes/icarus/_config.yml中打开搜索

```objc
insight: true // 打开内部搜索
```
+ 安装内部搜索支持

```objc
~ npm install -S hexo-generator-json-content
```
+ 重新部署/ 启动服务  
 
## 技巧
部署到github以后hexo只是把相应生成blog的静态文件上传到了相应分支上而非源文件，我们要在其他电脑重新部署时无法使用源文件解决这个问题可以从两方面：

+ 新建一个git仓库存放源文件
+ 在当前仓库创建一个新分支存放源文件

## 未完待续...

