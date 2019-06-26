---
title: SourceTree如何跳过注册直接使用
toc: true
date: 2019-06-25 12:34:11
tags:
- Mac
categories:
- 开发工具
type:
---



# Mac上SourceTree免注册使用方式

​		为什么我会总结这篇文章，想必不用多少。SourceTree的注册问题一直是个头疼的问题，一次无意中发现这么个骚操作，预计记录下来。话不多少，开始正题。

### 1. 下载应用

​		自行下载SourceTree应用，也可以在该链接下载。[SourceTree2.7.6](https://pan.baidu.com/s/1qKhbs8hZvRa8CypESLb01w) 密码:gjg6

### 2. 安装应用	

​		安装下载好的程序，知道这一步

​		![1](1.png)

​		然后点击取消安装，关闭SourceTree

### 3. 删除文件

​		划重点，关键步骤。

​		右键应用程序中SourceTree，显示包内容，然后在`Contents`文件夹中搜索`Atlassian`，结果如图

​		![1](2.png)

​		最后一个操作，删除搜索到的所有文件，整个流程结束。

​		可以愉快的使用SourceTree了。