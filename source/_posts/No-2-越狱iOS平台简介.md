---
title: No.2 越狱iOS平台简介
toc: true
date: 2019-10-17 23:07:55
tags:
- iOS
categories:
- 逆向工程
---

# 越狱iOS平台简介

## 1. iOS系统结构

常规渠道的App，苹果开放的能直接访问iOS系统的接口非常有限，开发者只需要根据文档完成开发即可。

因为权限极低，AppStore的App不能访问自身目录以外的绝大多数文件；而iOS一但越狱，来自Cydia的App就可以拥有比普通App更高的权限。

因为逆向的对象来自iOS，所以访问iOS全系统文件时开展iOS逆向工程的首要前提。

### 1.1 iOS目录结构简介

Filesystem Hierarchy Standard（以下简称FHS）为类UNIX操作系统的文件目录结构制定了一套标准，它的初衷之一是让用户预知文件或目录的存放位置。OSX在此基础上形成了自己的hier(7)框架。类UNIX操作系统的常见目录结构如下所示。

    ·/：根目录，以斜杠表示，其他所有文件和目录在根目录下展开。
    ·/bin：“binary”的简写，存放提供用户级基础功能的二进制文件，如ls、ps等。
    ·/boot：存放能使系统成功启动的所有文件。iOS中此目录为空。
    ·/dev：“device”的简写，存放BSD设备文件。每个文件代表系统的一个块设备或字符设备，一般来说，“块设备”以块为单位传输数据，如硬盘；而“字符设备”以字符为单位传输数据，如调制解调器。
    ·/sbin：“system binaries”的简写，存放提供系统级基础功能的二进制文件，如netstat、reboot等。
    ·/etc：“Et Cetera”的简写，存放系统脚本及配置文件，如passwd、hosts等。在iOS中，/etc是一个符号链接，实际指向/private/etc。
    ·/lib：存放系统库文件、内核模块及设备驱动等。iOS中此目录为空。
    ·/mnt：“mount”的简写，存放临时的文件系统挂载点。iOS中此目录为空。
    ·/private：存放两个目录，分别是/private/etc和/private/var。
    ·/tmp：临时目录。在iOS中，/tmp是一个符号链接，实际指向/private/var/tmp。
    ·/usr：包含了大多数用户工具和程序。/usr/bin包含那些/bin和/sbin中未出现的基础功能，如nm、killall等；/usr/include包含所有的标准C头文件；/usr/lib存放库文件。
    ·/var：“variable”的简写，存放一些经常更改的文件，比如日志、用户数据、临时文件等。其中/var/mobile和/var/root分别存放了mobile用户和root用户的文件，是重点关注的目录。
上述目录中的内容多用于系统底层，逆向难度较大，作为初学者，暂时不用在其中投入太多精力。建议初学者从学和练的角度出发，循序渐进，由易到难，先从熟悉的内容开刀，这样效率更高。

作为iOS开发者，日常操作所对应的功能模块大多来自iOS的独有目录，如下所示。

    ·/Applications：存放所有的系统App和来自于Cydia的App，不包括StoreApp。
    ·/Developer：如果一台设备连接Xcode后被指定为调试用机（如图2-4所示），Xcode就会在iOS中生成这个目录，其中会含有一些调试需要的工具和数据。
    ·/Library：存放一些提供系统支持的数据。其中/Library/MobileSubstrate下存放了所有基于CydiaSubstrate（原名MobileSubstrate）的插件。
    ·/System/Library：iOS文件系统中最重要的目录之一，存放大量系统组件，其目录结构如图2-7所示。
在逆向工程的初学阶段，需要重点关注`·/System/Library`

### 1.2 iOS文件权限简介

iOS是一个多用户操作系统。“用户”是一个抽象的概念，代表对操作系统的所有权和使用权；“组”使用户的一种组织方式，一个组可以包含多个用户，一个用户也可以属于多个组。

iOS中的每一个文件都有一个属主用户和一个属主组，每个文件都有一系列的权限。iOS用3位（bit）来表示文件的权限，iOS用3位（bit）来表示文件的权限，从高位到低位分别是r（read）权限、w（write）权限，以及x（execute）权限。文件与用户的关系存在以下三种可能性：

    * 此用户是属主用户；
    * 此用户不是属主用户，但在属主组里；
    * 此用户既不是属主用户，又不在属主组里。

所以需要用3*3位来表示一个文件的权限，如果某一位为1，则这一位代表的权限生效，否则无效。例如，111101101代表rwxr-xr-x，即该文件的属主用户拥有r、w、x权限，而属主组和其他所有人只具有r和x权限；同时，二进制的111101101转换成十六进制是755，也是一种常见的权限表示法。

## 2. iOS二进制文件类型

iOS逆向初学阶段，我们目标是是Application、Dynamic Library（简称dylib）和Daemon这三类二进制文件，对它们的了解越深入，逆向工程就会越顺利。

### 2.1 Application

#### 2.1.1 bundle

bundle的概念来源于NeXTSTEP,它不是一个文件，而是一个按照某种标准来组织的一个目录，其中包含了二进制文件以及运行所需的资源。

普通APP中的framework是以bundle形式存在的；越狱iOS中常见的PerferenceBundle本质也是bundle。Framework也是bundle，但framework的bundle中存放的是一个dylib，而不是可执行文件。

#### 2.1.2 App目录结构

1. **Info.plist**

    记录APP基本信息

2. **可执行文件**
3. **Lproj**
  
    lproj目录下存放的是各种本地化的字符串(.strings)

#### 2.1.3 系统App VS.StoreApp
1. 目录结构
2. 安装包格式与权限
3. 沙盒（sandbox）
  
### 2.2 Dynamic Library

### 2.3 Daemon