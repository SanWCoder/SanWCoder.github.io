---
title: load跟initalize区别
comments: true
date: 2018-04-09 11:43:42
tags:
 - Object-C
categories:
 - iOS
photos: /gallery/+load（）调用流程图.png
thumbnail: /gallery/+load（）调用流程图.png
---
<font color=#31AEE7 size=5 face=“宋体”>initialize</font> 和 <font color=#31AEE7 size=5 face=“宋体”> load </font>是 NSObject 的两个特殊类方法，在面试过程中很有可能会问到同时在工作中我们也可以利用其相应的特性做一些操作。

<!-- more -->  

## load 

### 基本区别
1. load是在程序运行时出发（加入编译文件Compile Sources），且仅调用一次，无需手动调用。
2. load 方法不遵从继承规则，如果类本身没有实现load方法，那么系统就不会调用，谁实现就会调用谁的load方法。
3. 尽可能的精简load方法，因为整个应用程序在执行load方法时会阻塞，即，程序会阻塞直到所有类的load方法执行完毕，才会继续
4. load 方法中最常用的就是方法交换method swizzling

### 实例分析
开发文档说明：
![load API](/gallery/load API.png)
 1. 创建类及其load方法

```objc
// People.m
+ (void)load{NSLog(@"class == People,method == load");}
// Student.m 继承自People
+ (void)load{NSLog(@"class == Student,method == load");}
// Teacher.m 继承自People
+ (void)load{NSLog(@"class == Teacher,method == load");}
// People+Ext.m People的分类
+ (void)load{NSLog(@"class == People (Ext),method == load");}
 
// method swizzling 实现方法交换
+ (void)load{
    Method personMethod = class_getInstanceMethod([Person class], NSSelectorFromString(@"personSay"));
    
     Method studentMethod = class_getInstanceMethod([Student class], NSSelectorFromString(@"studentSay"));

    method_exchangeImplementations(personMethod, studentMethod);
    }
```

2. 运行结果

![load终端调用结果](/gallery/load终端调用顺序.png)    


## initalize

### 基本区别

1. 当某个类第一次收到消息的时候触发,且只调用一次，无需手动调用。
2. 父类实现initalize，子类没有实现，调用子类会同时触发父类两次
3. 父类实现initalize，子类也实现，调用子类会先触发父类再触发子类
4. 父类实现initalize，父类分类也实现，分类方法会覆盖父类方法，调用的是分类方法

### 实例分析

开发文档说明：
![initialize API](/gallery/initialize API.png)

#### 创建类及其initialize方法
 
```objc
// People.m
+ (void)initialize{NSLog(@"class == %@,method == initialize",NSStringFromClass([self class]));}
// Student.m 继承自People
+ (void)initialize{NSLog(@"class == %@,method == initialize",NSStringFromClass([self class]));}
// People+Ext.m People的分类
+ (void)initialize{NSLog(@"class categary == %@,method == initialize",NSStringFromClass([self class]));}
```

#### 运行结果

![initalize终端调用结果](/gallery/initalize终端调用顺序.png)


