---
title: 利用RunLoop监控卡顿
toc: true
date: 2019-08-20 07:19:51
tags:
- Objective-C
categories:
- iOS
---

## 导致卡顿的原因

* 复杂 UI 、图文混排的绘制量过大；
* 在主线程上做网络同步请求；
* 在主线程做大量的 IO 操作；
* 运算量过大，CPU 持续高占用；
* 死锁和主子线程抢锁。

## RunLoop原理

RunLoop 是 iOS 开发中的一个基础概念，为了帮助你理解并用好这个对象，接下来我会先和你介绍一下它可以做哪些事儿，以及它为什么可以做成这些事儿。

RunLoop 这个对象，在 iOS 里由 CFRunLoop 实现。简单来说，RunLoop 是用来监听输入源，进行调度处理的。这里的输入源可以是输入设备、网络、周期性或者延迟时间、异步回调。RunLoop 会接收两种类型的输入源：一种是来自另一个线程或者来自不同应用的异步消息；另一种是来自预订时间或者重复间隔的同步事件。

RunLoop 的目的是，当有事件要去处理时保持线程忙，当没有事件要处理时让线程进入休眠。所以，了解 RunLoop 原理不光能够运用到监控卡顿上，还可以提高用户的交互体验。通过将那些繁重而不紧急会大量占用 CPU 的任务（比如图片加载），放到空闲的 RunLoop 模式里执行，就可以避开在 UITrackingRunLoopMode 这个 RunLoop 模式时是执行。UITrackingRunLoopMode 是用户进行滚动操作时会切换到的 RunLoop 模式，避免在这个 RunLoop 模式执行繁重的 CPU 任务，就能避免影响用户交互操作上体验。

**RunLoop执行原理**

1. 通知 observers：RunLoop 要开始进入 loop 了。紧接着就进入 loop。
2. 开启一个 do while 来保活线程。通知 Observers：RunLoop 会触发 Timer 回调、Source0 回调，接着执行加入的 block。
3. 回调触发后，通知 Observers：RunLoop 的线程将进入休眠（sleep）状态。
4. 进入休眠后，会等待 mach_port 的消息，以再次唤醒。只有在下面四个事件出现时才会被再次唤醒：
   * 基于 port 的 Source 事件；
   * Timer 时间到；
   * RunLoop 超时；
   * 被调用者唤醒。

5. 唤醒时通知 Observer：RunLoop 的线程刚刚被唤醒了。
6. RunLoop 被唤醒后就要开始处理消息了：
   * 如果是 Timer 时间到的话，就触发 Timer 的回调；
   * 如果是 dispatch 的话，就执行 block；
   * 如果是 source1 事件的话，就处理这个事件。
   
   消息执行完后，就执行加到 loop 里的 block。

7. 根据当前 RunLoop 的状态来判断是否需要走下一个 loop。当被外部强制停止或 loop 超时时，就不继续下一个 loop 了，否则继续走下一个 loop 。

**整个过程示意图**

![1](1.png) 

