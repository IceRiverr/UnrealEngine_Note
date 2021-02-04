> 初稿：2020年09月24日

## UE4 的单位
UE4 的长度单位为厘米。

1U = 1cm。

## Debug UE4
VS 中**调试--附加到进程**，可以用于调试 UE4 的代码部分。

将引擎编译一个 Debug Editor 就可以 debug 编辑器。

使用debug模式可以直接dubug游戏。

## 蓝图编辑器使用技巧
- 蓝图窗口的导航栏，有 收藏/先前导航/向后导航，并且右键可以看到导航历史。
- Ctrl+P，可以快速浏览蓝图和资源，方便读蓝图代码
- FindResults 窗口，通过放大镜，可以搜索所有的蓝图节点，类似 VS 的全局搜索
- F9 可以打断点。
- 蓝图 Debug 时，在当前蓝图工具栏，选择 Play，在 DebugFilter 上选择对应的蓝图进行 Debug。
- 在蓝图的 Window/Developer Tools/Blueprint Debugger，可以打开调用堆栈。
- 蓝图中从父类集成的变量，通过 Class Defaults 来设定默认值。
- 通过 ClassSettings/Interfaces 给蓝图类添加接口。

[Blueprint Debugging](https://docs.unrealengine.com/en-US/Engine/Blueprints/UserGuide/Debugging/index.html)

## PIE 模式
编辑器状态下，按下 F8，可以点选大纲中的 Actor，查看其属性值。

## 关卡编辑器 Viewport

移动摄像机的快捷方式，
- 右键 + A/W/S/D 来前后左右移动
- 右键 + Q/E 上下移动。
- 右键 + 滚动鼠标滚轮，调整移动的速度。

Ctrl+Alt+LMB，可以框选场景中的角色。可以方便的选取 Trigger 和 Collision。

选中Actor，按下 End，悬空的物体会自动贴到地面。

在场景中选中 Pawn，按下 Ctrl+B，可以在内容浏览器中定位资源。

Ctrl+P，打开一个 Asset Picker，用来选择蓝图和资源。并且可以添加 Filters 的过滤器。

PlayFromHere 可以从当前选中的地方开始玩，加快了迭代速度。

将 Play 设置成，SpawnActorAt.. CurrentCameraLocation，这样可以快速测试，在当前摄像机位置生成角色。

Actor 的 HiddenInGame 可以将碰撞盒在场景中使用。

按下 G 进入游戏模式，可以影藏编辑器图标和框图。

## 内容浏览器 Content Browser

通过 Filters 可以添加不同的物品种类标签，比如 DataAsset、DataTable、StaticMesh，方便过滤。

选中资源，按下 Ctrl+E，可以快速打开该资源对应的编辑器。

将 Content Browser 设置不同你的颜色会好很多。

在 ContentBrowser 中按下 F2 可以快速改名。

## 蓝图
快捷键
- 按住 B，然后点击，可以快速产生一个 Branch 的蓝图节点。
- 按住 S，然后点击，快速产生一个 Sequence 蓝图节点。
- Ctrl 挪动连接线。
- Alt 断开连接线。

在蓝图函数和事件
- 可以使用 LocalVariable，优化蓝图
- 将蓝图变量 *ExposeOnSpawn* 可以将变量在 ConstructScript 中暴露出来。
- 将要设置的变量直接连接到 CustomEvent 的输出端上，就可以在自定义事件上自动添加一个输出 pin。
- 在蓝图函数中，对于参数的引用，可以直接连线，也可以直接输入参数名，右键输入 GetXXX 来读取参数。

将一些蓝图节点塌陷成宏，不会增加类似函数的调用费用，又可以使蓝图节点优化。

使用 Reference Viewer 可以间查看资源的相互依赖情况。

当想要 Cast 某个值时，直接输入目标蓝图的名字，更快。比如 BP_Box，只输入 BP 就可以过滤一堆。

输入 `Input Key + F` 可以快速找到按键。

创建函数时，如果没有输出，比如要添加一个 bool 输出，可以将这个输出线拉到函数名上，就可以快速添加一个输出。