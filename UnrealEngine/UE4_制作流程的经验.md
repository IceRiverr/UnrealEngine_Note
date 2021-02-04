> 初稿：2020年10月21日

## 编辑器配置
将其他来源的资源导入我们的项目，最好准备一个专门供导资源的工程。要使用的资源现在曾 ContentDump 的项目中准备好，再迁移到我们的主项目中。
- 方便版本管理。
- Migrate->主项目的 Content，只能是这个目录。

在 Editor Preferences/General/Apperance/Editor Main Window Background Override 中设置 Tint 可以修改编辑器的外观颜色。

## VS 配置

[Unreal Engine + Visual Assist](https://www.youtube.com/watch?v=HfB-b4aYZcI)
使用 VS + 番茄插件 来配置 UE4 的开发。

https://www.wholetomato.com/visual-assist-ue4-unreal-engine

[Rider for Unreal Engine](https://www.jetbrains.com/lp/rider-unreal/)，
这是一个编辑器，对 UE4 的开发进行了优化，可以试一试。

## 版本管理
尝试一下 UE4 提供的默认版本管理机制。

Shared DDC，[DDC](https://docs.unrealengine.com/en-US/Engine/Basics/DerivedDataCache/index.html)

Perforce，不知道怎么来配置和使用？【问题】

## 工具

https://blueprintue.com/ ，可以改显示 UE4 导出导出的文本蓝图。

## 项目测试
[Automation Technical Guide](https://docs.unrealengine.com/en-US/Programming/Automation/TechnicalGuide/index.html) 
Programmer's guide to creating engine-level Automation Tests.

UE4 官方提供了一部分自动化测试的建议，有空看一下。

## unlua
https://github.com/Tencent/UnLua

知乎上有一篇，讲 unlua 是怎么绑定到 UE4 的。

[UE4 热更新：基于 UnLua 的 Lua 编程指南](https://imzlp.me/posts/36659/)

## latent function

## UnrealVersionSelector
在离线状态下，点击安装，就可让 UE4 项目切换版本了。

