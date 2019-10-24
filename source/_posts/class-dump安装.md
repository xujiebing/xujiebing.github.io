---
title: class-dump安装
toc: true
date: 2019-10-24 21:35:40
tags:
categories:
- 开发工具
---

# class-dump安装

由于Mac系统10.11 引入安全防护程序Rootless有些系统目录中不允许添加文件 但是class-dump需要在每个文件夹下均可以正常使用 所以就需要配置环境变量 下面是10.12安装class-dump的方法

## 1.下载

[class-dump官网下载](http://stevenygard.com/projects/class-dump/)

将下载好的包解压，解压之后有一个`class-dump`文件，这就是我们的目标文件

## 2.安装

在终端中按顺序执行

```
cd ~/
mkdir bin
sudo -s
cd ~/bin
```

将`class-dump`复制到`bin`文件夹下，继续终端中执行

```
chmod 777 class-dump
cd ~/
vim ~/.bash_profile
```

在编辑区域将`export PATH=$HOME/bin/:$PATH`复制到最后一行

最后执行

```
source ~/.bash_profile
```

安装结束

## 3.测试

终端输入

```
class-dump
```

输出结果结果为下面所示，既安装成功

```
class-dump 3.5 (64 bit)
Usage: class-dump [options] <mach-o-file>

  where options are:
        -a             show instance variable offsets
        -A             show implementation addresses
        --arch <arch>  choose a specific architecture from a universal binary (ppc, ppc64, i386, x86_64, armv6, armv7, armv7s, arm64)
        -C <regex>     only display classes matching regular expression
        -f <str>       find string in method name
        -H             generate header files in current directory, or directory specified with -o
        -I             sort classes, categories, and protocols by inheritance (overrides -s)
        -o <dir>       output directory used for -H
        -r             recursively expand frameworks and fixed VM shared libraries
        -s             sort classes and categories by name
        -S             sort methods by name
        -t             suppress header in output, for testing
        --list-arches  list the arches in the file, then exit
        --sdk-ios      specify iOS SDK version (will look in /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS<version>.sdk
        --sdk-mac      specify Mac OS X version (will look in /Developer/SDKs/MacOSX<version>.sdk
        --sdk-root     specify the full SDK root path (or use --sdk-ios/--sdk-mac for a shortcut)
```

