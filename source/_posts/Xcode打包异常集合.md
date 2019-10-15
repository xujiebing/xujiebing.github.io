---
title: Xcode打包异常集合
toc: true
date: 2019-06-24 22:10:47
tags:
- Xcode
categories:
- 开发工具
type:
---

#### 1. AppStore上传鉴定时报错

Xcode上传AppStore验证时报错：

> Unable to download a software component: com.apple.transporter.mediatoolkit/1.13.0

这个错误是下载jar包失败，按照以下步骤执行，执行完之后，重新上传验证

```
cd ~
mv .itmstransporter/.old_itmstransporter/
"/Applications/Xcode.app/Contents/Applications/Application Loader.app/Contents/itms/bin/iTMSTransporter"
```



