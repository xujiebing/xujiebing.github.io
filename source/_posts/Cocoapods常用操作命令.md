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

* 安装`podfile`中的远程库

  ```
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

  

