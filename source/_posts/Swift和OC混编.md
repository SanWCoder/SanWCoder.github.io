---
title: Swift和OC混编
comments: true
date: 2018-02-10 14:05:43
tags:
 - Object-C
 - Swift
categories:
 - iOS
photos:
thumbnail:
---

在OC项目中添加Swift文件进行混编

<!-- more -->

## 创建桥接文件  

### 新建类language选择swift

![1](/gallery/混编-001.png)

### 创建 项目名-Bridging-Header.h 的桥接文件 

继续下一步会有如下提示，点击创建，意思是在oc项目中使用Swift语言需要创建桥接文件默认桥接文件名// 项目名-Bridging-Header.h 此文件中存放要在swift文件中用到的OC文件的头文件即Swift调用OC

![2](/gallery/混编-002.png)

手动更改默认的桥接文件

![3](/gallery/混编-003.png)  

## 调用  

### Swift调用OC
 

![4](/gallery/混编-004.png)

![5](/gallery/混编-005.png)

### OC调用Swift

OC调用Swift和Swift调用OC相似，Swift调用OC是通过桥接文件中定义的头文件将相应的OC类转化为Swift类使用，而OC调用Swift需要引入系统自动生成的项目名-Swift.h文件 // #import "项目名-Swift.h",会默认将Swift类转化为OC类供OC调用，此文件在项目中是看不见的，但导入以后会看到相应的内容。
注:OC 能调用的Swift类，必须继承自NSObject或NSObject的派生类才能使用

![6](/gallery/混编-006.png)

项目名-Swift.h文件内容

![7](/gallery/混编-007.png)

![8](/gallery/混编-008.png)

我们也可以更改项目名称，导入#import "修改后的名-Swift.h"

![9](/gallery/混编-009.png)



