---
title: iOS推送
comments: true
date: 2018-04-10 12:15:57
tags:
 - iOS
 - 推送
categories:
 - iOS
photos:
 - /gallery/APNS.jpg
thumbnail:
 - /gallery/APNS.jpg
---


我们的产品是一款新闻类的APP要求按照用户行为推送相应的新闻数据，所以要用到推送，在程序启动的时候会请求用户是否打开推送，如果打开则能收到推送的新闻内容，相应的功能按钮也将打开，如果没有打开用户可以在更多设置里去手动打开，在我们的程序中并没有通过打开和关闭按钮去打开推送，而是让用户跳到系统的设置页面去自己选择是否打开。

<!-- more -->

##  推送流程

1. 设置开关状态值

```objc
// 设置开关状态
self.switchView.on = [[NSUserDefaults standardUserDefaults] boolForKey:@"push"];
```

2. 判断是否打开推送

```objc
/**
* 判断是否打开推送
*/
+ (BOOL)isAllowedNotification {
  // iOS8 check if user allow notification
  if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 8.0) {
    // system is iOS8
    UIUserNotificationSettings *setting = [[UIApplication sharedApplication] currentUserNotificationSettings];
    if (UIUserNotificationTypeNone != setting.types) {
      return YES;
    }
  } else {
    //iOS7
    UIRemoteNotificationType type = [[UIApplication sharedApplication] enabledRemoteNotificationTypes];
    if(UIRemoteNotificationTypeNone != type)
      return YES;
  }
  return NO;
}
```

3. 跳转到APP的系统设置界面

```objc
 // 跳转到APP的系统设置界面
 if (switchView.isOn) {
      NSURL * url = [NSURL URLWithString:UIApplicationOpenSettingsURLString];
      if([[UIApplication sharedApplication] canOpenURL:url]) {
        NSURL*url =[NSURL URLWithString:UIApplicationOpenSettingsURLString];     
 [[UIApplication sharedApplication] openURL:url];
      }
   }
   ```
## 解决思路
一开始对于这个功能一点也没有具体的解决思路，而且一直在程序里去思考这个问题，当用户跳到系统设置界面以后不管打开没有打开再次进入到程序中时没法知道是否打开也没法更新数据，之后突然想到当跳到系统设置界面的时候，程序已经进入了后台，自此有了我的解决方案。

1. 当用户跳转到系统设置界面以后程序进入了后台，而当再次进入时程序由后台进入到前台复原状态，由此我们可以在applicationDidBecomeActive时发送一个通知。

```objc
/**
 当程序进入后台，再返回时注册发送通知
*/
- (void)applicationDidBecomeActive:(UIApplication *)application
{
  // 发送通知
  [[NSNotificationCenter defaultCenter] postNotificationName:@"pushNoti" object:nil];
}
```

2. 之后再需要更新数据的控制器里注册通知，当收到通知时重新判断是否打开了推送，然后更新数据。
 
```objc 
/**
* 在viewDidLoad里注册通知
*/
- (void)viewDidLoad {
  [super viewDidLoad];
     // 是否打开通知使用NSUserDefaults存储
  [[NSUserDefaults standardUserDefaults] setBool:[WEFileUtils isAllowedNotification] forKey:@"push"];
   // 注册通知
  [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(updataPush) name:@"pushNoti" object:nil];
}
/**
* 收到通知以后重新取值存储然后更新数据
*/
- (void)updataPush{
  [[NSUserDefaults standardUserDefaults] setBool:[WEFileUtils isAllowedNotification] forKey:@"push"];
  [self.tableView reloadData];
}
```

## 结束语

自此应用内打开推送功能集成成功，当然目前新闻推送目前还没有完美的方案去推送数据，同时这只是个人的一些思路和解决方案，有不足的地方还望提出来一起交流。


