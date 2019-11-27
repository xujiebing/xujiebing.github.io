---
title: 禁用Apple Pay
toc: true
date: 2019-11-27 07:26:05
tags:
- Objctive-C
categories:
- iOS
---

# 禁用Apple Pay

本篇文章主要介绍如何屏蔽iOS手机弹出Apple Pay页面。

## 业务场景

当iPhone打开Apple Pay功能，并靠近具备NFC识别的设备时，iPhone的Apple Pay会被自动唤起，在屏幕上弹出一个Apple Pay页面。大部分的业务场景下并没有什么不妥，但是今天我们这个业务场景下，用户体验会非常糟糕。

某一天你去杭州、上海或者其他能够使用二维码进出站的城市坐地铁，当你把手机屏幕上的二维码对准闸机，准备迈开腿进站的时候，闸机没开，平移手机，希望闸机能尽快读到二维码，这样你就可以进站了。然而，这并没有什么卵用，闸机毫无反应。你拿起手机一看，我了个叉，一个Apple Pay页面遮挡在二维码页面上面，闸机根本识别不到二维码，能进站才是见鬼了。

需求已经明确，我们就要考虑下怎么实现了。

## 需求分析

通过分析Apple Pay页面，以及查看Apple Pay相关知识，发现弹出的页面是一个`PKAddPassesViewController`,且一个iPhone同时只能弹出一个`PKAddPassesViewController`，也就是说，当iPhone的Apple Pay，即`PKAddPassesViewController`页面，被唤起之后，不能再唤起其他任何的`PKAddPassesViewController`页面。根据这个现象，分析出具体的实现思路。

## 实现思路

1. 在需要禁用Apple Pay的页面弹出一个`PKAddPassesViewController`页面，然后将`hidden`属性设置为`NO`
2. 结束
3. 就是这么简单粗暴

## 源码实现

[DBInvalidApplePay](https://github.com/xujiebing/DBInvalidApplePay)

已发布到Cocoapods，可直接：

```ruby
pod 'DBInvalidApplePay'
```

## 代码示例

```objective-c
// 屏蔽Apple Pay
[DBInvalidApplePay dbInValidApplePay];

// 取消屏蔽Apple Pay
[DBInvalidApplePay dbValidApplePay];
```

