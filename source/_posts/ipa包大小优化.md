---
title: ipa包大小优化
toc: true
date: 2019-07-30 06:45:03
tags:
- 学习笔记-iOS
categories:
- iOS
---

## 官方 App Thinning

App Thinning 是由苹果公司推出的一项可以改善 App 下载进程的新技术，主要是为了解决用户下载 App 耗费过高流量的问题，同时还可以节省用户 iOS 设备的存储空间。

App Thinning 有三种方式，包括：App Slicing、Bitcode、On-Demand Resources。

* App Slicing，会在你向 iTunes Connect 上传 App 后，对 App 做切割，创建不同的变体，这样就可以适用到不同的设备。
* On-Demand Resources，主要是为游戏多关卡场景服务的。它会根据用户的关卡进度下载随后几个关卡的资源，并且已经过关的资源也会被删掉，这样就可以减少初装 App 的包大小。
* Bitcode ，是针对特定设备进行包大小优化，优化不明显。

## 无用图片资源

删除无用图片的过程，可以概括为下面这 6 大步。

1. 通过 find 命令获取 App 安装包中的所有资源文件，比如 find /Users/daiming/Project/ -name。
2. 设置用到的资源的类型，比如 jpg、gif、png、webp。
3. 使用正则匹配在源码中找出使用到的资源名，比如 pattern = @"@"(.+?)""。
4. 使用 find 命令找到的所有资源文件，再去掉代码中使用到的资源文件，剩下的就是无用资源了。
5. 对于按照规则设置的资源名，我们需要在匹配使用资源的正则表达式里添加相应的规则，比如 @“image_%d”。
6. 确认无用资源后，就可以对这些无用资源执行删除操作了。这个删除操作，你可以使用 NSFileManger 系统类提供的功能来完成。

> 工具[LSUnusedResources](https://github.com/tinymind/LSUnusedResources)

## 图片资源压缩

对于 App 来说，图片资源总会在安装包里占个大头儿。对它们最好的处理，就是在不损失图片质量的前提下尽可能地作压缩。目前比较好的压缩方案是，将图片转成 WebP。[WebP](https://developers.google.com/speed/webp/) 是 Google 公司的一个开源项目。

**为什么选择WebP**

* WebP 压缩率高，而且肉眼看不出差异，同时支持有损和无损两种压缩模式。比如，将 Gif 图转为 Animated WebP ，有损压缩模式下可减少 64% 大小，无损压缩模式下可减少 19% 大小。
* WebP 支持 Alpha 透明和 24-bit 颜色数，不会像 PNG8 那样因为色彩不够而出现毛边。

## 代码瘦身

App 的安装包主要是由资源和可执行文件组成的，所以我们在掌握了对图片资源的处理方式后，需要再一起来看看对可执行文件的瘦身方法。

可执行文件就是 Mach-O 文件，其大小是由代码量决定的。通常情况下，对可执行文件进行瘦身，就是找到并删除无用代码的过程。而查找无用代码时，我们可以按照找无用图片的思路，即：

* 首先，找出方法和类的全集；
* 然后，找到使用过的方法和类；
* 接下来，取二者的差集得到无用代码；
* 最后，由人工确认无用代码可删除后，进行删除即可。

## 通过 AppCode 找出无用代码

**检测结果说明**

* 无用类：Unused class 是无用类，Unused import statement 是无用类引入声明，Unused property 是无用的属性；
* 无用方法：Unused method 是无用的方法，Unused parameter 是无用参数，Unused instance variable 是无用的实例变量，Unused local variable 是无用的局部变量，Unused value 是无用的值；
* 无用宏：Unused macro 是无用的宏。
* 无用全局：Unused global declaration 是无用全局声明。

**AppCode 静态检查的问题：**

* JSONModel 里定义了未使用的协议会被判定为无用协议；
* 如果子类使用了父类的方法，父类的这个方法不会被认为使用了；
* 通过点的方式使用属性，该属性会被认为没有使用；
* 使用 performSelector 方式调用的方法也检查不出来，比如 self performSelector:@selector(arrivalRefreshTime)；
* 运行时声明类的情况检查不出来。比如通过 NSClassFromString 方式调用的类会被查出为没有使用的类，比如 layerClass = NSClassFromString(@“SMFloatLayer”)。还有以 [[self class] accessToken] 这样不指定类名的方式使用的类，会被认为该类没有被使用。像 UITableView 的自定义的 Cell 使用 registerClass，这样的情况也会认为这个 Cell 没有被使用。

> 基于以上种种原因，使用 AppCode 检查出来的无用代码，还需要人工二次确认才能够安全删除掉。

