---
title: UUID解决方案
comments: true
date: 2016-06-12 13:31:47
tags:
categories:
photos:
thumbnail:
---

在项目中用到了UUID，需要在用户第一次安装以后再次安装时UUID不会再变化，作为一个唯一的标识，就这个问题找了一些资料，记录下来以便下次使用。

<!-- more -->




## 简介

参考来源 http://www.tuicool.com/articles/Ir63UbI

UUID解决方案主要采用了keychain存储的解决方案，我们知道如果使用NSUserDefaults来存储的话当卸载以后数据也将会一并删除，目前最好的解决方案就是使用keychain存储。

## 创建keychain

![创建keychain流程](/gallery/UUID-001.png)
创建完以后会自动生成一个entitlements文件  
![生成的entitlements文件](/gallery/UUID-002.png)

## 自定义存储类

```objc
  #import "INKeyChainStore.h"
  @implementation INKeyChainStore
+ (NSMutableDictionary *)getKeychainQuery:(NSString *)service {
    return [NSMutableDictionary dictionaryWithObjectsAndKeys:
            (id)kSecClassGenericPassword,(id)kSecClass,
            service, (id)kSecAttrService,
            service, (id)kSecAttrAccount,
            (id)kSecAttrAccessibleAfterFirstUnlock,(id)kSecAttrAccessible,
            nil];
}
// 将值保存在keychain中
+ (void)save:(NSString *)service data:(id)data {
    NSMutableDictionary *keychainQuery = [self getKeychainQuery:service];
    SecItemDelete((CFDictionaryRef)keychainQuery);

    [keychainQuery setObject:[NSKeyedArchiver archivedDataWithRootObject:data] forKey:(id)kSecValueData];
    SecItemAdd((CFDictionaryRef)keychainQuery, NULL);
}
// 获取存进入的值
+ (id)load:(NSString *)service {
    id ret = nil;
    NSMutableDictionary *keychainQuery = [self getKeychainQuery:service];

    [keychainQuery setObject:(id)kCFBooleanTrue forKey:(id)kSecReturnData];
    [keychainQuery setObject:(id)kSecMatchLimitOne forKey:(id)kSecMatchLimit];
    CFDataRef keyData = NULL;
    if (SecItemCopyMatching((CFDictionaryRef)keychainQuery, (CFTypeRef *)&keyData) == noErr) {
        @try {
            ret = [NSKeyedUnarchiver unarchiveObjectWithData:(__bridge NSData *)keyData];
        } @catch (NSException *e) {

        } @finally {

        }
    }
    if (keyData)
        CFRelease(keyData);
    return ret;
}
// 删除值
+ (void)deleteKeyData:(NSString *)service {
    NSMutableDictionary *keychainQuery = [self getKeychainQuery:service];
    SecItemDelete((CFDictionaryRef)keychainQuery);
}

@end
```

## 自定义一个类来获取UUID

```objc
//  获取设备UUID
+ (NSString *)getUUID
{
    NSString * strUUID = (NSString *)[INKeyChainStore load:@"uuid.yaowen"];

    //首次执行该方法时，uuid为空
    if ([strUUID isEqualToString:@""] || !strUUID)
    {
        //  生成一个uuid的方法
        CFUUIDRef uuidRef = CFUUIDCreate(kCFAllocatorDefault);
        strUUID = (NSString *)CFBridgingRelease(CFUUIDCreateString (kCFAllocatorDefault,uuidRef));
        strUUID =  [strUUID stringByReplacingOccurrencesOfString:@"-" withString:@""];
        // 将该uuid保存到keychain
        [INKeyChainStore save:@"uuid.yaowen" data:strUUID];
    }
    return strUUID;
}
```

