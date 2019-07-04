---
title: ReactNative开发踩坑记录
toc: true
date: 2019-07-03 10:48:58
tags:
- React
categories:
- ReactNative
---

### Could not find iPhone XXX 

**描述：**

终端运行`react-native run-ios`时报错，无法找到模拟器，通过`xcrun simctl list devices`命令查看本地模拟器列表，已经包含了运行的模拟器

**原因：**

不能再通过cli运行`react-native run-ios`

**解决方案：**

进入`/node_modules/react-native/local-cli/runIOS/findMatchingSimulator.js`文件,将

```
if (!version.startsWith('iOS') && !version.startsWith('tvOS'))
```

修改为

```
if (!version.startsWith('com.apple.CoreSimulator.SimRuntime.iOS') && !version.startsWith('com.apple.CoreSimulator.SimRuntime.tvOS'))
```



