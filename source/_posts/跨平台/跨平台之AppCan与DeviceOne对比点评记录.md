---
title: 跨平台之AppCan与DeviceOne对比点评记录
date: 2016-07-14 09:50:44
img: /images/20160714.png
top: true
categories: 跨平台
tags:
  - 跨平台
  - AppCan
  - DeviceOne
---

目前已有的移动中间件开发技术主要包括：`IOS`、`Android`或`WindowsPhone`的纯原生开发；以`Html5`技术为核心的中间件开发（例如：`PhoneGap`, `HBuilder`, `AppCan`, `ApiCloud`）；以`OpenGL`技术为核心的中间件开发(例如：`CrossApp`)；以代码转换和原生反射技术为核心的中间件开发(例如：`Titanium`，`Xamarin`，`ReactNative`)；以及以虚拟UI、抽象SDK、动态组件为核心的中间件开发（例如：`DeviceOne`）。

本次主要拿 `AppCan` 与 `DeviceOne` 做对比，说下它们的共同点、不同点和各自的特性

### `AppCan` 与 `DeviceOne`共同点和不同点：

#### 共同点：
1. 跨平台，都支持目前的三大主流系统：Android,iOS,WindowsPhone
2. 都是以提取Eclipse开发工具的基本功能并稍作修改后，集成自己的IDE来给开发者一个独立的开发工具。

#### 不同点：
1. `AppCan`主要采用`HTML5`+`CSS3`+`JS`来开发
2. `DeviceOne`主要采用`JS`或`Lua`来开发

### `AppCan` 与 `DeviceOne`各自特性：

### AppCan:
#### 核心：
    以`html5技术`为核心的中间件开发。（主推`Hybrid`模式）

#### 优点：
1. 支持四大主流系统：`Android`,`iOS`,`Symbian`,`WindowsPhone`
2. 提供一体化解决方案，方便环境搭建、开发、调试、发布
3. 框架自带UI包，包含常用控件样式
4. 框架API丰富
5. 支持本地打包、云端打包
6. 基于密钥的代码加密
7. 框架对UI、动画渲染进行过优化，反应速度比纯html5快

#### 缺点：
1. 不开源，无法修改、优化底层代码
2. 暂不支持自行开发控件，无法调取android原生功能
3. 框架自带功能过多，导致应用安装包偏大。
4. 部分系统无法使用IDE进行调试
5. 只能在服务器端发布，无法在本地发布（即代码需上传至服务器才能发布）
6. IOS发布，需要将证书上传至服务器

### DeviceOne:

#### 核心：
    以`虚拟UI`、`抽象SDK`、`动态组件`为核心的中间件开发。

#### 优点：
1. 支持三大主流系统：`Android`,`iOS`,`WindowsPhone`
2. UI布局可拖拽
3. 屏幕自动适配
4. UI是原生的
5. 开放的组件商店，可自定义组件

#### 缺点：
1. 不支持本地打包，需要远程（服务器）打包
2. 组件商店的公共组件太少
3. 推出时间太短，市场检验的时间还够
4. 相关的文档资料太少

本次的`AppCan`与`DeviceOne`对比点评就到这里~


