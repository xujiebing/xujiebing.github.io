---
title: Cocoapods常用操作命令
toc: true
date: 2019-06-25 23:37:46
tags:
- CocoaPods
categories:
- 开发工具
type:
---



## 1.Pod安装

* 创建Podfile文件

  ```ruby
  pod init
  ```

* 安装`podfile`中的远程库

  ```ruby
  pod install
  ```

* 安装`podfile`中的远程库，但不更新本地spec文件

## 2.缓存相关

* 查看所有spec文件的缓存，可以直接到路径下删除文件

  ```ruby
  pod cache list
  ```

* 删除制定的缓存文件

  ```ruby
  pod cache clean AFNetworking
  ```

## 3.踩坑记录

* **[!] Attempt to read non existent folder '/Users/xxx'**

  检查一下上述报错路径中是否包含`中文`，如果有请更改为全英文路径再重新`pod install`

