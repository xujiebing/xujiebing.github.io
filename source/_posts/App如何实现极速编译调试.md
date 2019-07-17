---
title: App如何实现极速编译调试
toc: true
date: 2019-07-18 07:07:19
tags:
- 学习笔记-iOS
categories:
- iOS
---

虽然动态链接器的实际应用能够帮助我们缩短App编译时间，但是每次修改代码之后还是需要重新启动App，再走一遍调试流程。对于我们开发人员来说，提高编译调试的速度就是提高生产力。那么，目前有哪些工具是可以实现动态调试的呢？

## Swift Playground

Playground是 Xcode 里集成的一个能够快速、实时调试程序的工具，可以实现所见即所得的效果，如下图所示：

![](1.png)

## Flutter Hot Reload

Flutter 是 Google 开发的一个跨平台开发框架，调试也是快速实时的。官方的效果动画如下：

![](2.gif)

## Injection for Xcode

