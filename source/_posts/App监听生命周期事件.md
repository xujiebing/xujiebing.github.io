---
title: App监听生命周期事件
toc: true
date: 2019-11-18 06:52:31
tags:
- Objective-C
categories:
- iOS
---

# App监听生命周期事件

在App开发过程中，我们经常需要监听App的生命周期。一般情况下是通过注册监听，接收系统的通知，处理通知结果。

**实现三步曲：**

```objective-c
1.添加监听
  [NSNotificationCenter.defaultCenter addObserver:self selector:@selector(p_didFinishLaunching:) name:UIApplicationDidFinishLaunchingNotification object:nil];

2.定义接收函数
  - (void)p_didFinishLaunching:(NSNotification *)obj {
    // TODO:处理业务代码
}

3.销毁监听
  - (void)dealloc {
    [NSNotificationCenter.defaultCenter removeObserver:self];
}
```

可以看出，整个过程虽然比较简单，但是步骤较多，每次监听都这么写比较浪费时间。于是本篇文章的主角诞生。

引入`AJAppEvent`库

```shell
pod 'AJAppEvent'
```

**实现代码：**

```objective-c
[AJAppEvent didFinishLaunching:^(AJAppEventModel * _Nonnull eventModel) {
        NSLog(@"%@", eventModel.name);
    }];
```

一步搞定，是不是效率高很多。

**实现思路：**

[AJAppEvent](https://github.com/xujiebing/AJAppEvent)库实现并非有什么难点，只是将简单重复的事情集中统一处理，提高开发效率。源码请点击[链接](https://github.com/xujiebing/AJAppEvent)

`AJAppEvent`中支持一下通知类型：

```
// 应用程序进入后台
UIApplicationDidEnterBackgroundNotification
// 应用程序将要进入前台
UIApplicationWillEnterForegroundNotification
// 应用程序完成启动
UIApplicationDidFinishLaunchingNotification
// 应用程序由挂起变的活跃
UIApplicationDidBecomeActiveNotification
// 应用程序挂起(有电话进来或者锁屏)
UIApplicationWillResignActiveNotification
// 应用程序终止(后台杀死、手机关机等)
UIApplicationWillTerminateNotification
// 应用程序收到内存警告
UIApplicationDidReceiveMemoryWarningNotification
// 当有重大时间改变(凌晨0点，设备时间被修改，时区改变等)
UIApplicationSignificantTimeChangeNotification
// 设备方向将要改变
UIApplicationWillChangeStatusBarOrientationNotification
// 设备方向改变
UIApplicationDidChangeStatusBarOrientationNotification
// 设备状态栏frame将要改变
UIApplicationWillChangeStatusBarFrameNotification
// 设备状态栏frame改变
UIApplicationDidChangeStatusBarFrameNotification
// 应用程序在后台下载内容的状态发生变化
UIApplicationBackgroundRefreshStatusDidChangeNotification
// 本地受保护的文件被锁定,无法访问
UIApplicationProtectedDataWillBecomeUnavailable
// 本地受保护的文件可用了
UIApplicationProtectedDataDidBecomeAvailable
```

