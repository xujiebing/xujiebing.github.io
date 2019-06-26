---
title: Cocoapods创建podspec说明
toc: true
date: 2019-06-22 22:58:57
tags:
- CocoaPods
categories:
- 开发工具
type:
---

## 一.创建远程私有库 

创建一个私有的podspec包括如下那么几个步骤: 

```
- 创建并设置一个私有的Spec Repo。 
- 创建Pod所需要的项目工程文件。
- 向远程仓库提交工程项目。
- 向私有的Spec Repo中提交podspec
- 在个人项目中引入私有库。 
- 更新维护podspec。 
```



在整个过程中我们一共需要两个Git仓库。一个是用来放Pods索引的，也就是第一步中用到的，而且只有在第一次创建远程库时才需要；另一个是用来存放工程代码的远程仓库，不一定是Git仓库，其它的远程仓库也可以，本文以Git来介绍。 

### 1. 创建并设置一个私有的Spec Repo 

Spec Repo是所有Pods的一个索引，相当于一个容器，所有的Pods都在这个索引里面。如 `~/CocoaPods/Specs`是所有公共的Pods的索引，稍后我们创建BWTSpecs就是我们所有Pods私有库的索引。 

接下来我们创建Spec Repo，在终端执行以下命令： 

```
pod repo add BWTSpecs http://git.bwton.com/msx-client-ios/BWTSpecs.git
```

http://git.bwton.com/msx-client-ios/BWTSpecs.git 是我们创建好的存放BWTSpecs索引的仓库。 

成功之后在` ~/.cocoapods/repos`目录下会多出一个BWTSpecs文件。第一步完成。 

> PS:多人开发时，其他成员也需要执行这个命令，且其他成员需要有这个Git仓库的权限。 

### 2. 创建Pod所需要的项目工程文件 

在终端进入需要创建项目的目录，然后执行： 

```
pod lib create BWTKit 
```

执行完之后会有几个问题，按需要回答即可。回答完之后会自动执行pod install命令创建项目并生成依赖。 

现在进入到项目目录下的Example目录下，打开workspace文件，删掉Replaceme文件，开始编辑BWTKit.podspec文件，参考以下格式： 

```
Pod::Spec.new do |s| 
s.name = 'BWTKit' 
s.version = '1.0.0' 
s.summary = '八维通基础业务模块' 
s.description = <<-DESC 
‘该模块主要处理码上行App中基础业务相关.’ 
DESC 
s.homepage = 'http://git.bwton.com' 
s.license = { :type => 'MIT', :file => 'LICENSE' } 
s.author = { 'xujiebing' => 'xujiebing1992@gmail.com' } 
s.source = { :git => 'http://git.bwton.com/msx-client-ios/BWTKit.git', :tag => s.version.to_s } 
s.ios.deployment_target = '8.0' 
s.source_files = 'BWTKit/*' 
end 
```

添加项目需要的代码，本地先测试编译，确保编译通过。 

### 3. 向远程仓库提交工程项目 

在终端中进入BWTKit目录 

将本地工程文件提交到远程仓库。执行以下命令： 

```
git add . 
git commit -m '初始化项目' 
git remote add origin http://git.bwton.com/msx-client-ios/BWTKit.git # 关联远程仓库 
git push origin master # 提交到远程仓库 
```

> PS：如果push失败可以尝试命令：git push origin master -f # 覆盖远程仓库 

podspec文件中需要获取Git的tag，所以我们这里需要打上tag。执行以下命令： 

```
git tag -m '第一个tag' 1.0.0 
git push --tags # 推送tag到远程仓库
```

### 4. 向私有的Spec Repo中提交podspec 

在终端中进入BWTKit目录 

先本地验证podspec文件，执行命令：

```
pod lib lint  
```

当看到以下输出表示本地验证通过 

```
-> BWTKit (1.0.0) 
BWTKit passed validation. 
```

接下来进行远程验证，执行命令： 

```
pod spec lint 
```

当看到以下输出表示远程验证通过 

```
-> BWTKit (1.0.0) 
BWTKit passed validation. 
```

验证完之后向远程BWTSpecs提交podspec，执行命令： 

```
pod repo push BWTSpecs BWTKit.podspec 
```

完成之后这个BWTKit库就添加到我们的私有BWTSpecs中了，可以进入到~/.cocoapods/repos/BWTSpecs目录下查看，也可以去远程仓库查看。 

### 5. 在个人项目中引入私有库 

在需要引入私有库的项目的podfile文件中引入： 

```
source 'https://github.com/CocoaPods/Specs.git' 
source 'http://git.bwton.com/msx-client-ios/BWTSpecs.git' 
pod 'BWTKit', '~> 1.0.0'
```

> PS: source 'https://github.com/CocoaPods/Specs.git' 必须要加，否则项目中引入的共有的Pods库将无法安装 

### 6. 更新维护podspec 

我们已经制作好BWTKit 1.0.0版本，现在业务有变更，需要对BWTKit进行升级。 

这是只需要将2，3，4，5步中的version和tag设置成新的值，然后重复2，3，4，5步即可。 

## 二.远程私有库中引入私有库 

以BWTBaseBiz引入BWTKit为例 

### 1. 创建Pod所需要的项目工程文件 

在终端进入需要创建项目的目录，然后执行： 

```
pod lib create BWTBaseBiz 
```

执行完之后会有几个问题，按需要回答即可。回答完之后会自动执行pod install命令创建项目并生成依赖。 

现在进入到项目目录下的Example目录下，打开workspace文件，删掉Replaceme文件，开始编辑BWTBaseBiz.podspec文件，参考以下格式： 

```
Pod::Spec.new do |s| 
s.name = 'BWTBaseBiz' 
s.version = '1.0.0' 
s.summary = '八维通基础业务模块' 
s.description = <<-DESC 
‘该模块主要处理码上行App中基础业务相关.’ 
DESC 
s.homepage = 'http://git.bwton.com' 
s.license = { :type => 'MIT', :file => 'LICENSE' } 
s.author = { 'xujiebing' => 'xujiebing1992@gmail.com' } 
s.source = { :git => 'http://git.bwton.com/msx-client-ios/BWTBaseBiz.git', :tag => s.version.to_s } 
s.ios.deployment_target = '8.0' 
s.source_files = 'BWTBaseBiz/*' 
s.dependency 'BWTKit' # 注意：这一步比较关键 
end 
```

在Podfile中加入：# 注意：这一步有所区别 

```
source 'https://github.com/CocoaPods/Specs.git' 
source 'http://git.bwton.com/msx-client-ios/BWTSpecs.git' 
```

进入Example目录下执行 `pod install `

添加项目需要的代码，本地先测试编译，确保编译通过。 

### 2. 向远程仓库提交工程项目 

在终端中进入BWTBaseBiz目录 

将本地工程文件提交到远程仓库。执行以下命令： 

```
git add . 
git commit -m '初始化项目' 
git remote add origin http://git.bwton.com/msx-client-ios/BWTBaseBiz.git # 关联远程仓库 
git push origin master # 提交到远程仓库 
```

> PS：如果push失败可以尝试命令：git push origin master -f # 覆盖远程仓库 

podspec文件中需要获取Git的tag，所以我们这里需要打上tag。执行以下命令： 

```
git tag -m '第一个tag' 1.0.0 
git push --tags # 推送tag到远程仓库 
```

### 3. 向私有的Spec Repo中提交podspec 

在终端中进入BWTKit目录 

先本地验证podspec文件，执行命令：# 注意：这一步有区别 

```
pod lib lint --sources=http://git.bwton.com/msx-client-ios/BWTSpecs.git,https://github.com/CocoaPods/Specs.git 
```

当看到以下输出表示本地验证通过 

```
-> BWTKit (1.0.0) 
BWTKit passed validation. 
```

接下来进行远程验证，执行命令：# 注意：这一步有区别 

```
pod spec lint --sources=http://git.bwton.com/msx-client-ios/BWTSpecs.git,https://github.com/CocoaPods/Specs.git 
```

当看到以下输出表示远程验证通过 

```
-> BWTKit (1.0.0) 
BWTKit passed validation. 
```

验证完之后向远程BWTSpecs提交podspec，执行命令：# 注意：这一步有区别 

```
pod repo push BWTSpecs BWTKit.podspec --sources=http://git.bwton.com/msx-client-ios/BWTSpecs.git,https://github.com/CocoaPods/Specs.git 
```

完成之后这个BWTKit库就添加到我们的私有BWTSpecs中了，可以进入到~/.cocoapods/repos/BWTSpecs目录下查看，也可以去远程仓库查看。私有库引入私有库制作完毕。 

### 4. 在个人项目中引入私有库 

在需要引入私有库的项目的podfile文件中引入： 

```
source 'https://github.com/CocoaPods/Specs.git' 
source 'http://git.bwton.com/msx-client-ios/BWTSpecs.git' 
pod 'BWTBaseBiz', '~> 1.0.0' 
```

这时BWTKit和BWTBaseBiz都会被引入到项目中 

## 三.更新私有库 

搜索不到远程私有库时，可以执行以下操作： 

1.进入终端执行命令： 

```
rm ~/Library/Caches/CocoaPods/search_index.json 
```

2.进入到本地索引文件目录`~/.cocoapods/repos/BWTSpecs`下执行命令： 

```
git pull 
```

接着搜索私有库即可搜到 

## 四.如何删除私有库 

### 1. 删除整个BWTSpecs文件，执行命令： 

```
pod repo remove BWTSpecs 
```

这样本地的就删除了，还可以通过以下命令加回来： 

```
pod repo add BWTSpecs http://git.bwton.com/msx-client-ios/BWTSpecs.git 
```

### 2. 删除某个私有库 

进入目录 `~/.cocoapods/repos/BWTSpecs`下,删除私有库目录，执行命令： 

```
rm -Rf BWTKit 
```

然后将本地的修改推到远程库，执行命令： 

```
git add -A 
git commit -m '删除BWTKit私有库' 
git push origin master 
```

操作完毕 