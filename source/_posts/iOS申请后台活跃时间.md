---
title: iOS申请后台活跃时间
toc: true
date: 2019-07-05 06:48:15
tags:
categories:
- iOS
---

由于苹果的后台机制，当我们按下home键的时候，所有线程包括主线程的任务都会被挂起，一些资源比如socket也会被系统回收，会导致很多问题，比如一个很重要的资源中断下载，或者定时器方法被暂停等等。



苹果在4.0以后提供了一种申请后台时间的机制：

```
(UIBackgroundTaskIdentifier)beginBackgroundTaskWithExpirationHandler:(void (^)(void))handler
```

> 声明：

标记要开始一个新的长期运行的后台任务

> 参数：

handler 

应用程序后台剩余时间快到达为0的时候的一个处理回调，你应该使用这个回调来做一些清理工作和后台任务结束的标记，未能明确地结束任务将导致APP的终止，这个处理回调将在主线程中被同步调用。

> 返回值：

一个新的后台任务的唯一的标示符，你必须将这个值传给endBackgroundTask:方法来标记任务的结束。如果无法在后台运行这个方法将返回UIBackgroundTaskInvalid。

> 描述：

这个方法会让你的app转到后台以后继续运行一段时间，你可以在一个任务未完成将会导致影响用户体验的情况下调用此方法。例如，你可以调用次方法来获取足够的时间来传输一个很重要的文件到远程服务器或者至少尝试标记一些错误。你不应该随意的调用这个方法来保持你的app在后台长期运行。



实现比较简单，[点我查看Demo](https://github.com/xujiebing/BackgroundTask)