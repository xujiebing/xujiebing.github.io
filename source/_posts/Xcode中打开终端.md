---
title: Xcode中打开终端
toc: true
date: 2019-12-24 23:37:32
tags:
- Xcode
categories:
- 开发工具
---

本篇主要讲实现Xcode中直接打开终端，并定位到当前工程文件目录。

### 1. 新建.sh文件

```shell
#!/bin/sh
if [ -n "$XcodeProjectPath" ]; then
	open -a Terminal "$XcodeProjectPath"/..
else
	open -a Terminal "$XcodeWorkspacePath"/..
fi
```

### 2.配置Xcode

1. 打开`Xcode`->`设置`->`Behaviors`，选择左下角“+”，添加自定义选项，如图所示

![1](1.png)

2. 选择第一步中创建的.sh文件，若文件时灰色不可点击，可执行

```
chmod 777 XXXX.sh
```

3. 勾选run，否则脚本无法生效
4. 配置快捷键，上面截图中我配置的是`Command + T`,根据个人习惯配置
5. 配置完成，可以使用了