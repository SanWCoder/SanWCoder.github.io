---
title: cocospod相关操作
comments: true
date: 2017-06-18 14:26:59
tags:
 - cocospod
categories:
 - iOS
photos:
thumbnail:
---

在项目中使用cocospod管理第三方库的一些基本操作

<!-- more -->


## 创建podfile文件  

  `pod init`
  
## 安装第三方库  

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

## 安装  

```objc
/// 此时会生成 .lock文件
pod install
```
## cocospod 更新
   
### 先切换gem源  

```objc
gem sources --remove https://rubygems.org/`
gem source -a https://gems.ruby-china.org  
// 查看是否切换成功
gem source -l

打印出*** CURRENT SOURCES ***

           https://gems.ruby-china.org

就说明切换成功
```
### 升级  

`sudo gem install -n /usr/local/bin cocoapods --pre`  

### 查看版本  

`pod --version`  

### 设置cocospod仓库  

```objc
pod repo update
// pod setup
```


