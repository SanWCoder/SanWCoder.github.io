---
title: cocospod相关操作
comments: true
date: 2018-04-10 14:26:59
tags:
 - cocospod
categories:
 - iOS
photos:
thumbnail:
---

在项目中使用cocospod管理第三方库的一些基本操作

<!-- more -->

## cocospod 基本操作

* 创建podfile文件  

  `pod init`
  
* 安装第三方库  

```objc
// 在podfile中添加第三方库如
# Uncomment this line to define a global platform for your project
platform :ios, '9.0'
use_frameworks!

target 'LiteraryHeaven' do
pod 'SVProgressHUD'
 pod 'Heimdall', '~> 1.0.0'
# pod 'ReactiveCocoa','~> 4.0.0'
end
```

* 安装  

```objc
pod install
```
* cocospod 更新
   
1> 先切换gem源  

```objc
gem sources --remove https://rubygems.org/`
gem source -a https://gems.ruby-china.org  
// 查看是否切换成功
gem source -l

打印出*** CURRENT SOURCES ***

           https://gems.ruby-china.org

就说明切换成功
```
2>升级  

`sudo gem install -n /usr/local/bin cocoapods --pre`  

3> 查看版本  

`pod --version`  

4> 设置cocospod仓库  

```objc
pod repo update
// pod setup
```


