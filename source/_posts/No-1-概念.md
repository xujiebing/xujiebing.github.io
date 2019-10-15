---
title: No.1 概念
toc: true
date: 2019-10-16 06:58:55
tags:
categories:
- 逆向工程
---

## 1. iOS应用逆向工程的作用

iOS逆向工程主要有两个作用：

* 分析目标程序，拿到关键信息，可以归类于<font color=#FF4500 size=3 face="黑体">安全相关的逆向工程</font>
* 借鉴他人的程序功能来开发自己的软件，可以归类于<font color=#FF4500 size=3 face="黑体">开发相关的逆向工程</font>

### 1.1 安全相关的逆向工程
1. 评定安全等级
2. 逆向恶意软件
3. 检查软件后门
4. 去除软件使用限制

### 1.2 开发相关的逆向工程
1. 逆向系统API
2. 借鉴别的软件

## 2. iOS应用逆向工程的过程
1. 系统分析
2. 代码分析

## 3. iOS应用逆向工程的工具
iOS逆向工程的工具主要分为四大类：
1. 监测工具
2. 反汇编工具（disassembler）
3. 调试工具（debugger）
4. 开发工具

### 3.1 监测工具
1. Reveal
2. snoop-it
3. introspy

### 3.2 反汇编工具
1. IDA
    支持Windows、Linux、OSX平台和多种处理器架构
2. Hopper
    主要针对苹果操作系统

### 3.3 调试工具
iOS逆向过程中主要使用的是LLDB

### 3.4 开发工具
1. Xcode
2. Theos