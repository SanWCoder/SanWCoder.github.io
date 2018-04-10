---
title: iOS代码编写规范
comments: true
date: 2018-04-10 11:20:45
tags:
 - code rule
categories:
 - iOS
photos:
thumbnail:
---

代码的规范对于开发而言是十分重要的同时也是衡量一名程序猿合格的指标，这篇文章写下关于自己对于iOS编写的习惯及一些理解。

<!-- more -->

#### 一.注释规范

##### 1.创建类添加类注释
```objc
/**
 * <#空格#>SanW--2017.5.3--公共参数
 */
@interface PTCommonParams : NSObject
```
##### 2.属性注释
```objc
/**
 *<#空格#>设备系统版本
 */
(/// <#空格#>设备系统版本)
@property<#空格#> (nonatomic,copy)<#空格#>NSString<#空格#>*osVersion;
```
##### 3.更改对方代码时打上注释
```objc
#warning mark<#空格#>- SanW--2017.5.3--解决数组越界奔溃（原因）
```
##### 4.创建变量
```objc
 // <#空格#>是否退出（变量以下划线_开头）
BOOL<#空格#>_isQuit;
```
#### 二.命名规范（驼峰命名法） 
> 1.It is good to be both clear and brief as possible, but clarity shouldn’t suffer because of brevity:
2.In general, don’t abbreviate names of things. Spell them out, even if they’re long:
3.However, a handful of abbreviations are truly common and have a long history of use. You can continue to use them; see Acceptable Abbreviations and Acronyms.
4.Avoid ambiguity in API names, such as method names that could be interpreted in more than one way.  

##### 1.类，方法，属性，变量命名
```objc
// <#空格#>类命名一般添加个人/公司/应用程序大写字母作为前缀
PTHoneController,PTUserModel,PTHeaderView
// <#空格#>方法命名主要从 --要什么 -- 做什么
-<#空格#>(NSString<#空格#>*)itemNamed:(NSString<#空格#>*)name      
-<#空格#>(NSString<#空格#>*)findItemWithName:(NSString<#空格#>*)name  
-<#空格#>(NSString<#空格#>*)itemAtIndex:(NSUInteger)index 
//<#空格#>变量名称以下划线开头_
BOOL<#空格#>_isQuit;
```
##### 2.枚举表示状态、选项
*定义的枚举类型名称应以 2~3 个大写字母开头，而这通常与项目设置的类文件前缀相同，跟随其后的命名应采用驼峰命名法则，命名应准确表述枚举表示的意义，枚举中各个值都应以定义的枚举类型开头，其后跟随各个枚举值对应的状态、选项或者状态码。*
```objc
// 名称以类名前缀+表示的意义 值：枚举名+对应的状态、选项或者状态码
typedef NS_ENUM(NSInteger,ReportHeaderType) {
ReportHeaderTypeAudio, //<#空格#>音频
ReportHeaderTypeVideo //<#空格#>视频
};
typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {
UIViewAutoresizingNone = 0,
UIViewAutoresizingFlexibleLeftMargin = 1 << 0,
UIViewAutoresizingFlexibleWidth = 1 << 1,
UIViewAutoresizingFlexibleRightMargin = 1 << 2,
UIViewAutoresizingFlexibleTopMargin = 1 << 3
};
```
##### 3.常量
**最好以宏代替部分方法，常量用const去定义**
* 1> 使用宏预处理定义以k+代表意义定义，这样定义的常量无类型信息，且如果你在调试时想要查看 kRadius 的值却无从下手，因为在预处理阶段kRadius 就已经被替换了，不便于调试
//<#空格#>圆角弧度
`#define kRadius 6  `
* 2> 使用类型常量为常量带入类型信息
```objc
// <#空格#>通常以个人/公司/类名大写字母作为前缀+代表的意义+Const后缀
const<#空格#>NSTimeInterval<#空格#>PTAnimationDurationConst<#空格#>=<#空格#>0.3;
NSString<#空格#>*const<#空格#>UIApplicationLaunchOptionsLocalNotificationKey;
// <#空格#>消息区分私信
.h
extern<#空格#>NSString<#空格#>*const<#空格#>MESSAGE_LIVE_TYPE;
.m
NSString<#空格#>*const<#空格#>MESSAGE_LIVE_TYPE<#空格#>=<#空格#>@"直播间主播私信";
```
##### 4.分类及其分类方法
*分类名称以个人/公司/类名大写字母作为前缀+系统类名+ Ex后缀，分类中的方法名以个人/公司/类名小写字母作为前缀，且以下划线连接前缀与方法名。*
```objc
UIColor+PTColorEx.h
// <#空格#>16进制颜色转换
+<#空格#>(UIColor<#空格#>*)pt_colorWithHexString:(NSString<#空格#>*)hexString;
```
##### 5.block和协议命名
* Block

```objc
// <#空格#>block表示的意义首字母大写 + Block后缀
@property<#空格#>(nonatomic,copy)<#空格#>void<#空格#>(^ChooseClassfyBlock)(PTLiveClassfyModel<#空格#>*classfyInfo,UIButton<#空格#>*sender);
```
* 2.代理协议

```objc
// <#空格#>使用protocol定义代理协议时类名+Delegate 后缀
@protocol<#空格#>PTHomeViewDelegate<#空格#><NSObject>
```

##### 6.判断（switch/ if.....else）

*在项目中尽量减少if...else 的使用，使用switch便利，相对而言if条件判断，switch匹配*


