---
title: Mac Launchpad(启动台)图标大小调整
toc: true
date: 2019-10-17 18:12:13
tags:
- Mac
categories:
- Mac相关
---

打开终端（Terminal），输入一下命令

**1. 调整每页列数**

```
defaults write com.apple.dock springboard-columns -int 10
```

**2. 调整每页行数**

```
defaults write com.apple.dock springboard-rows -int 8
```

**3. 重置Launchpad**

```
defaults write com.apple.dock ResetLaunchPad -bool TRUE
```

**4. 重启Dock**

```
killall Dock
```

设置完毕，进入启动台查看效果

**5. 恢复默认设置**

```
defaults write com.apple.dock springboard-rows Default
defaults write com.apple.dock springboard-columns Default
defaults write com.apple.dock ResetLaunchPad -bool TRUE
killall Dock
```

