> 初稿：2020年09月05日

周六，办公室，我要用这个时间节点，把编辑器的扩展好好学一下。

组内正在进行这方面的开发，我需要进行更加系统的学习和整理。做好技术支持。

---

## 一些参考

[UE4项目记录（十一）自定义资源及](https://zhuanlan.zhihu.com/p/41332747)，实现了一个类似行为树的剧情编辑器。

## Editor Utility Widget
http://anapurna.co.jp/ue4/641/

[How to Make Tools in UE4](https://lxjk.github.io/2019/10/01/How-to-Make-Tools-in-U-E.html)

## TSubclassOf
TSubclassOf is a template class that provides UClass type safety. 

用来安全的提供 UClass，比如 TSubclassOf<UDamageType>，在编辑器中就会只出现 UDamageType 的子类，可以添加这种强制约束。

## UE4 控制台命令

[Running Unreal Engine](https://docs.unrealengine.com/en-US/GettingStarted/RunningUnrealEngine/index.html)

## UE4 架构
[UE4基础引擎架构](https://blog.csdn.net/weixin_39823748/article/details/78933822?utm_medium=distribute.pc_relevant_right.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant_right.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)

[UE4-Actors](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Actors/index.html)
- 这个文档非常重要。【重读】

[Unreal Property System (Reflection)](https://www.unrealengine.com/zh-CN/blog/unreal-property-system-reflection)

## UE4 模块

[Gameplay Modules](https://docs.unrealengine.com/en-US/Programming/Modules/Gameplay/index.html)
- 同一个游戏可以创建不同的模块，其中主模块为 IMPLEMENT_PRIMARY_GAME_MODULE，而其它模块使用 IMPLEMENT_GAME_MODULE。

[ue4 模块的构建和加载](https://www.cnblogs.com/wellbye/p/5837108.html)
- 这个文档从代码层级对模块进行讲解。【重读】

UE4 是使用模块构建起来的。一个 *.Build.cs 对应一个模块。

所有的模块都继承自 IModuleInterface。这个接口管理自己模块内部的加载和卸载。

UE4 中所有的模块，都被 FModuleManager 来管理，这个管理器是一个单例，并且可以通过 LoadModuleChecked(InModuleName) 的方式来加载需要的模块。

``` c++
FContentBrowserModule& COntentBrowserModule = FModuleManager::Get().LoadModuleChecked<FContentBrowserModule>("ContentBrowser");
```

[UE4模块系统详解](https://blog.csdn.net/pp1191375192/article/details/103139304)
- 如何创建不同的模块

[知乎-【UE4】插件与模块](https://zhuanlan.zhihu.com/p/121152396)

### ModuleRules

## 自动创建蓝图

如下的数组怎么使用，我还不清楚
- PublicDependencyModuleNames
- PrivateDependencyModuleNames
- DynamicallyLoadedModuleNames

## UE 编辑器开发
如下为B站的一些 UE4 与 python 使用教程
- [成为使用蓝图，C ++和Python的虚幻引擎自动化专家 - Tim Großmann](https://www.bilibili.com/video/BV1bT4y177v8?p=1)，是一个非常系统的使用 Python 来进行开发的教程，一共4个小时
- [【UE4】虚幻引擎使用Python开发 How To Use Python Inside Unreal Engine 4](https://www.bilibili.com/video/BV1b4411r7kX?from=search&seid=3861624276445703863)
- [Python in Unreal Engine _ Inside Unreal](https://www.bilibili.com/video/BV144411n7PW?from=search&seid=3861624276445703863)
- [UE4官方教程 现场讲座 使用Python简化资产工作流 Unreal Fest Europe 2019_压制版](https://www.bilibili.com/video/BV1p7411L74c?from=search&seid=3861624276445703863)
- [UEPy](https://space.bilibili.com/15247862/channel/detail?cid=124609)

一些重要博客：
- [Scripting the Editor using Python](https://docs.unrealengine.com/en-US/Engine/Editor/ScriptingAndAutomation/Python/index.html)
- [Unreal PythonAPI](https://docs.unrealengine.com/en-US/PythonAPI/index.html)

python 为 2.7 版本，为了和视效领域保持一致。即 https://vfxplatform.com/

## 学习 Python In Unreal Engine

2020年9月5日23:48:45，这两天我要投入时间，将 python 开发 UE4 的流程走通。

我喜欢这样一步一脚印的方式，慢慢来学习。

ue4 python 只是用来支持 editor，并不是为了 runtime gameplay logic。

观看视频为 [Python in Unreal Engine _ Inside Unreal](https://www.bilibili.com/video/BV144411n7PW?from=search&seid=3861624276445703863)。

一些好的建议：
- 在控制台，按上下键，可以在刚才输入的命令之间切换。
- 按 shift + Enter，可以在控制台输入多行。

import unreal
- UE4 的主要模块

print dir(unreal)
- 打印所有可用的模块
- 是 pythyon 的一个内置函数，用来获取 unreal 的所有内置属性。


help(dir)
- 是 python 内内置的帮助命令


chair_asset = unreal.load_asset('/Game/StarterContent/Props/SM_Chair.SM_Chair')
- 对 content 中的资产，右键--Copy Reference，可以获得 *StaticMesh'/Game/StarterContent/Props/SM_Chair.SM_Chair'*，这个参数方便加载资产。

unreal.log(chair_asset)
- 输出为： <Object '/Game/StarterContent/Props/SM_Chair.SM_Chair' (0x000002D0848C0800) Class 'StaticMesh'>

有了 **StaticMesh** 以后，可以到 PythonAPI 来进行搜索。

如果不愿意使用 API，可以直接使用如下的代码来查询：

for attribute in dir(chair_asset):
    print attribute

print help(chair_asset)
- 和上面有同样的效果，并且更细

prop = chair_asset.get_editor_property("lod_for_collision")
- 通过这种方式，可以获得编辑器属性
- 通过 |prop = chair_asset.set_editor_property("lod_for_collision", 0)| 可以切换到特定的值。

什么情况下应该使用上面的 get/set 方法？
- Python 中可以设置的属性 attributes，一定是蓝图暴露为 read-only 或者 readwrite。
- 对于一些 editany where，visiable anywhere，view anywhere 的，可以通过 get set 访问，这些被暴露为 editor property，但是没有暴露为 attributes。
- 使用 function，因为这些函数一般会调用一些 post change notification，会通知编辑器里其他的资源。比如上面修改 lod_for_collision 时，实际会重新 Build Static Mesh。
- 如果通过 attributes 来设置，可能并没这些操作。

set_editor_property(name, value, notify_mode=PropertyAccessChangeNotifyMode.DEFAULT) → None 
- set the value of any property visible to the editor, ensuring that the pre/post change notifications are called
- 这样设置，确保了 pre/post change 被通知到
- 这样的调用的结果，就好像你在 editor 修改属性一样。
- 而修改 attribute，就好像代码中直接调用设置一样，没有处理 pre/post


### 使用文本来存储 python script

```python
import sys
for path in sys.path:
    print path

```

```python
import os
os.startfile("D:/Program Files/Epic Games/UE_4.25/Engine/Content/Python")
os.makedirs("D:/Program Files/Epic Games/UE_4.25/Engine/Content/Python")

```
但是系统会报没有这个脚本。

os.startfile("D:/Program Files/Everything/")
- 打开某个文件

os.startfile("D:/Program Files/Everything/Everything.exe")
- 执行某个程序

我们在 D:/Program Files/Epic Games/UE_4.25/Engine/Content/Python 下创建了一个 my_python.py，但是 import my_python 却发现失效，原因是在编辑器启动时，这个文件夹还没有被识别。只能重启编辑器。

import python_test
reload(python_test)
- 这样可以加载自己写的 python_test 模块，然后调用其中的函数

init_unreal.py
- 放在引擎下的该名字的脚本，会在编辑器启动时调用。
- 和 Maya 和 max 的机制一样。
- 通过这个脚本就可以做很多的事情。
- UE4.20 更新记录：Added support for init_unreal.py start-up scripts. These can be placed in any known sys.path in Python, including the Content/Python folders we automatically add, and the UnrealEngine/Python directory under the user's home directory.
- 可以添加自定义的菜单。

python 脚本如果引用了别的 plugin 中的脚本，但是脚本却没有加载，就会报错，只需要找到依赖的脚本即可。

使用 python 脚本来自动导入骨骼模型。

```python
unreal.GlobalEditorUtilityBase.get_default_objects()
utility_base = unreal.GlobalEditorUtilityBase.get_default_object()
selected_asset = utility_base.get_selected_assets()
print selected_asset
# 执行这个操作时需要首先选择 Content 中的资源。
# [StaticMesh'"/Game/Geometry/Meshes/1M_Cube.1M_Cube"']
```

UE4 返回的数组是 Array，需要通过 list(arr)，转化为 python 的 list 来使用。
- 这种情况在 maya，max，houdini 的开发中很常见，这些软件会有一些 python 封装的 c++ 类，此时必须进行这样的转化。
- 可以通过 type(arr) 检查返回物品的种类。

