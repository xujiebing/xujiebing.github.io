---
title: Xcode导出ipa文件时四种类型说明
toc: true
date: 2019-06-21 19:45:19
tags:
- Xcode
categories:
- iOS
type:
---

在 iOS 导出 ipa包的时候 会有四个选项： 

1. Save for iOS App Store Deployment 保存到本地 准备上传App Store 或者在越狱的iOS设备上使用 
2. Save for Ad Hoc Deployment 保存到本地 准备在账号添加的可使用设备上使用（具体为在开发者账户下添加可用设备的udid），该app包是发布证书编译的（The app will be code signed with the distribution certificate.） 
3. Save for Enterprise Deployment 这种主要针对企业级账户下 准备本地服务器分发的app 
4. Save for Development Deployment 针对内部测试使用，主要给开发者的设备(具体也为在开发者账户下添加可用设备的udid)。该app包是开发证书编译的（The app will be code signed with your development certificate） 

