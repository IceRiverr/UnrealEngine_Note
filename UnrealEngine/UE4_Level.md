> 初稿：2020年11月29日

将关卡加载、关卡管理都放在这里。

## 参考
[流关卡与无缝地图切换-经验总结](https://zhuanlan.zhihu.com/p/34397446)

[UE4流关卡与无缝地图切换总结](https://blog.csdn.net/u012999985/article/details/78484511)

## StreamingLevel
[Level Streaming](https://docs.unrealengine.com/en-US/Engine/LevelStreaming/index.html)

ULevelStreaming
- `FName PackageNameToLoad;` 
- `TArray<FName> LODPackageNames;` 支持 Level 的 LOD 系统。很优雅的一种解法。无法蓝图设置。
- `int32 LevelLODIndex;` 当前 LOD 级别。可以设置。

如何给 Level 通过 LevelStreaming 添加 LOD 机制？【问题】

Asynchronously loading and unloading levels during play to decrease memory usage and create seamless worlds.

异步加载和卸载关卡到内存中，可减少内存占用，并创建无缝世界。

当 StreamingLevel 被设置为 Always Loaded，它会和 Persistent Level 一起加载，并且忽略关联的 Volume 以及蓝图控制的加载和卸载。
这么来设置，一般是为了将 Persistent Level 中的内容分到 SubLevel 中，作为层来使用。方便团队协作。

StreamingLevel 中一些测试结果
- PersistentLevel 中的 Actors 都会保留，其属性会一直保留，并且一直 Update
- StreamingLevel，如果将 StreamingMethod 设置为 AlwaysLoaded，则 Unload 以后不再 Update，但是对象的属性还是保留了。
- StreamingLevel，如果将 StreamingMethod 设置为 Blueprint，则 Unload 以后，再次 Load，会重新调用 BeginPlay。相当于重建了。
- 玩家角色在跨越不同的 StreamingLevel，其属性被保留。

加载和卸载 StreamingLevel 的方式
- LoadSteamingLevel(LevelName, MakeVisiableAfterLoad, ShouldBolckOnLoad)，这个接口还是使用 Name 的字符串来标记。
- UnloadStreamingLevel(LevelName, ShouldBolckOnLoad)
- ULevelStreaming* GetStreamingLevel(PackageName) PackageName 是 SubLevel 的文件名。然后可以检测 IsLevelLoaded()。
- 并且获取到 ULevelStreaming 后，可以调整其属性。或者调用 CreateInstance() 来创建一个当前 Level 的实例，然后控制其位置和可见性，这样可以创建程序化世界。

LoadLevelInstance()
- 可以在特定位置和角度 Stream In Level，可以创建同一个 Level 的多个实例。
- LevelName 不需要放在 Persistent 的 Levels List 中，但是为了打包成功，需要将这个关卡资源放在 ProjectSettings -> Packaging -> List of Maps 中。
- 返回一个 StreamingLevelObject，可以通过 SetShouldBeLoaded() 来控制加载和卸载。以及 SetShouldBeVisible() 来设置可见性。
- bug，播放 LevelSequence 时， Spawn 的没问题，但是非 Spawn 的播放就出错。

[Level Streaming Volumes](https://docs.unrealengine.com/en-US/BuildingWorlds/LevelStreaming/StreamingVolumes/index.html)
- 根据玩家的 viewport 的位置来地图流。第三人称模板中，是摄像机的位置，所以不是玩家的位置。第一人称模板中，是在头部。
- 工作原理如下：每一个 Level 都分配一组 Volumes。每一帧引擎会遍历每一个 Level 所关联的 Volumes，如果玩家的视点在起哄一个 Volume 中，则加载，如果不在任何一个里边，则标记为 unload，等待卸载。
- `UWorld::ProcessLevelStreamingVolumes`
- `stat streamingDetails` 可以显示当前的加载状况
- 注意事项
  - Volumes 必须放置在 Persistent Level 中。
  - 如果一个 Level 和 Volume 关联，其他的加载方式会失效。
  - 适用于分屏的场景。
- 需要在目标平台测试 volume-based streaming level，因为不同的平台加载时间不一样，这样就要细分自己关卡的粒度了。
- 卸载滞后，Hysteresis，通过 `Min Time Between Volume Unload Requests` 来调整，目前默认是 2s。
- 可以 Disable Volume 的加载。用处是，加入一扇门和室内的场景关联，正常情况，玩家到了门附近就应该加载， 但是如果门是锁着的，就不需要加载，这时可以通过 Disable 掉这个 Volume 来实现这个功能。

## UE4 中切换关卡
UE4 在切换 Level 时，是否会销毁 Pawn
- 我测试的结果是会。那么如何在不同场景之间切换时，保持角色的属性。
- 可以使用 Save Game 的机制。

UE4 中切换 Level，并可以保留 Pawn 属性的方法
- 使用 StreamingLeve 机制。将整个场景拆分为不同的 StreamingLevel，然后切换时，角色的属性保留。那我要测试不同 Actor 的属性是否还保留。
- 使用 SaveGame 机制，直接切换。
- 使用 数据库，在切换前保存到数据库中，切换后再恢复。

给 TriggerBox 在 LevelBlueprint 中绑定 Overlap 函数的两种方式
- 将 TriggerBox 拖入关卡蓝图，然后再 BeginPlay 中通过 BindEventTo_OnActorBeginOverlap() 来定义事件。
- 选中关卡中的 TriggerBox，然后再关卡蓝图中，RMB-->AddEventForTriggerBox-->Collision/AddOnActorBeginOverlap 来调用。

## OpenLevel
OpenLevel() 会完全切换一个管钱。

## CheckPoint

## 加载页面
