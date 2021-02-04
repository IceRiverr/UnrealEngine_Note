> 初稿：2020年10月23日

学习东西，就要完整系统。

---

## 词汇表
playback un（录音或录像的）回放

cutscene 过场动画

Scene/Take/Roll 在拍电影的小板上写的是什么意思？

## Sequencer

Sequencer 中的快捷键
- S，可以给 Transform 快速k帧。
- Shift + Q/W/E，为平移、缩放、旋转上单独 K 帧。
- Ctrl+G，到达想要的帧位置。
- “[ ]” ，表示当前帧从哪儿开始和结束。
- 先 Ctrl+G，找到位置，然后 ]，可以方便地设置镜头的长度。
- Shift + 拖动所选资源，即在 Sequence 中添加了一个 Spawned 的物品。

每一个 Sequence 都是自包含资源，可以脱离场景存在，可以引用 Content 中的资源，而不是一定要场景中的物体。

Spawned 类型的物品，当 Sequence 播放时，在场景中自动产生，结束时又会消失。方便在同一个场景中创建多种不同的动画。

Shot Track，提供了一个 Sequence 包含多个 Sequence 的机制。可以做一个总的 Master 的镜头，然后通过 Shot Track 将这些都添加进来。然后同时进行镜头制作和剪辑。

场景中的所有动画都相对于一个 Cube 来创建，有更多的可调整性。

对 Transform 进行设置轨迹时，右键\属性\Section\When Finished，有 KeepStete RestoreState 和 ProjectDefault，如果 KeepState 就可以将移动后的位移保持。

Shot Track 旁边的小摄像机，Lock Viewport to Shots，可以将 Cinematic Viewport 的摄像机改为当前 Sequence 指定的摄像机，非常方便。

当过场动画播放时，输入依然起作用，实际上可以将此时的输入禁止掉。

CineCamera 也有 ConstrainAspectRatio 的选项，将其不勾选，并可以k帧，这样就可以不显示黑边了。

Key Interpolation 插值方法，有 Linear 和 Constant。找一下区别。Constant 可以解决闪的问题。

Sequence 中的 Attach，比如 A Attach 到 B，是指 A 成为 B 的子级，这样方便在 Sequence 中保持相对关系。

GetSequenceBindind() 是一个神奇的节点，这是 UE4 专门写的功能。
- 先选择 Sequence 资源
- 然后选择需要绑定的 Actor。

但是这么做，每个资源都需要单独引用。我发现了一个新的方法，使用 Tag
- [Binding](https://docs.unrealengine.com/en-US/BlueprintAPI/Game/Cinematic/Bindings/index.html)


在设置 Animation Track 时，Animation Section 的左右上角可以调整，从而改变混合强度。【技巧】

## Sequence Binding
[Bindings](https://docs.unrealengine.com/en-US/BlueprintAPI/Game/Cinematic/Bindings/index.html)

[Actor Rebinding in Blueprints with Sequencer](https://docs.unrealengine.com/en-US/Engine/Sequencer/HowTo/AnimateDynamicObjects/index.html)

使用 GetSequenceBinding() 和 SetBinding() 可以将动画对齐。
- GetSequenceBinding(SequenceAssetRef, BindingObject) --> MovieSceneObjectBindingID，用于接下来 进行绑定。
- LevelSequencePlayerActor->AddBinding()/SetBinding(BindingID, NewActor)  使用这个 NewActor 可以替换 BindingObject 的实体。

AddBinding() 和 SetBinding() 的区别。
- Add 可以一个一个 Actor 的添加
- Set 需要设置一个数组

通过这种方式，就可以先用普通模型来 Key 动画，然后在运行时，切换为玩家角色模型，就可以在 Key 动画时，不用知道玩家的模型。

但是这种方式，SequenceAssetRef 不能作为参数使用，只有一个 Sequence 还好，一旦多了，每个都得手动 Binding，可能有问题。

UE4.24 中给 Sequence 提供了基于 Tag 的 Binding 机制
- 在 Sequence 编辑器中，选中轨道中的对象，右键-->Tags-->OpenBindingTagManager，可以右键，添加 Tag
- 在播 Sequence 时，可以通过 AddBindingByTag()/SetBindingByTag() 来直接绑定，并替换成自己希望的动作。

我测试的一个流程如下：
- 构造一个 BP_ProxyCharacter，其包含有 Capsule，Arrow，SkeletonMesh 作为组件，并且将 AnimationMode 设置为 UseAnimationAsset 类型。
- 在 Sequence 使用这个代理角色来制作剧情，并给其添加 Tag。并且将其 ActorHiddenInGame
- 在程序中，如果想要让 PlayerCharacter 替换这个 Sequence 中的角色，不能隐藏该 Player，并且通过 AddBindingByTag 来进行角色的绑定。

## Geometry Cache
将整个模拟在 maya 中制作好，然后导出为 abc，然后在 UE4 中使用 Geometry Cache 来进行布料模拟和破碎。

[Introduction to Alembic | Live Training | Unreal Engine](https://www.youtube.com/watch?v=rDHxxk0y2D4)

## InstanceData - Transform Origin
Sequence 播放的基点，这样 Sequence 中所有的 Absolute Transform Section 都会基于这个基点。不适用于 Relative 或 Additive sections。

一种设置的方式是，在场景中直接放入 LevelSequenceActor，然后再此处设置。

另一种方式是在蓝图中设置。
- LevelSequenceActor 中通过 bOverrideInstanceData 来开启和关闭该功能。
- LevelSequenceActor 中通过 `UObject* DefaultInstanceData` 来保存该变量。该变量是一个 BlueprintReadWrite 的成员。
- LevelSequenceActor 实现了 IMovieSceneTransformOrigin 接口。
- 设置方法为，通过这个接口，我对 UE4 的资源管理
  - LevelSequenceActor->DefaultInstanceData | Cast<UDefaultLevelSequenceInstanceData>，然后直接设置这个值。
  - Construct<UDefaultLevelSequenceInstanceData> | LevelSequenceActor->Set

一种感想是，只要是 UObject 类型的类型的成员，就可以被构造，然后用蓝图对这个 UObject 的成员进行 Set，然后将新构建的 UObject 对象设置给更上层的对象，比如 Actor。

我对组件模式的直观感受来自于看别人写的代码。

我对事件分发的直观感受来源于看别的项目。

我对场景的操作来源于对 UE4 提供的 StreamingLevel 接口的探索。然后感觉到 UE4 在我手中可以为所欲为。

## 一些问题

如何处理当前动画和 Sequencer 动画的融合？【问题】
1. 互不干扰，当玩家可控时，走玩家的动画蓝图。当播放 Sequence 时，需要再蓝图节点中添加 DefaultSlot 的槽，就可以支持 Sequence 提供的接口了。

在《超凡双生》中，Judy 从墙缝转出去时，动画怎么来做？【问题】

怎么来做当前镜头和 Sequencer 的镜头混合？

如何在 Sequencer 中开启两个窗口？【问题】

怎么进行设置比较方便？

如果在镜头中将 SensorAspeceRatio 设定好了，而游戏是或者16：9，或者4：3的分辨率，这就要对这些 AspectRatio 在开始时进行设置？【问题】


## 代码端
一个 Sequence 对应一个 ULevelSequence。播放动画的类 ULevelSequencePlayer。

ULevelSequence 继承自 UMovieSceneSequence。

ULevelSequencePlayer 继承自 UMovieSceneSequencePlayer。UMovieSceneSequencePlayer 是抽象类，为不同的动画播放器提供一致的播放行为。

UMovieSceneSequencePlayer 包含的核心函数有：
- Play()
- PlayReverse()
- ChangePlaybackDirection()
- PlayLooping()
- Pause()
- Scrub()，？
- Stop()
- StopAtCurrentTime()
- GoToEndAndStop()

UMovieSceneSequencePlayer 有一些 FOnMovieSceneSequencePlayerEvent 的事件：
- OnPlay
- OnPlayReverse
- OnStop
- OnPause
- OnFinished

在这些回调中可以对大量行为进行调整。

特别是在 OnFinished 的事件绑定中，可以处理大量的逻辑。
- 卡尔看电视，当电视看完时，发表了一段评论。这个就可以用 OnFinished 的机制实现。

## 架构
3DTransformSection/Track/Template 是 Transform 信息的轨道，可以好好看一下底层数据是怎么来做的。
- 包含有 floatChannel 来表示 float 中的一条轨道。

LevelVisibilitySection/Track/Template
- 整个结构比较复杂的地方在 Template 的计算上。

Template
- Setup() 构建
- TearDown() 拆卸

## 参考

[Sequencer Overview](https://docs.unrealengine.com/zh-CN/Engine/Sequencer/Overview/index.html)

[Cenematics With Sequencer](https://www.bilibili.com/video/BV1jt411y7Y1?p=1)

[Unreal Engine 4 Tutorial - Cutscenes - Level Sequencer](https://www.youtube.com/watch?v=dmqC7s91RBU)

[Unreal Engine 4: Creating events in sequencer for level blueprint(for 4.21-4.23)](https://www.youtube.com/watch?v=MZhs5UPU_oM)

[Sequencer - How To's](https://docs.unrealengine.com/en-US/Engine/Sequencer/HowTo/index.html) 
提供了大量和 Sequencer 编程有关的内容，值得重看。

[UE4 LevelSequence源码剖析（一）](https://zhuanlan.zhihu.com/p/157892605)

---

[Triggering Sequences from Gameplay](https://docs.unrealengine.com/en-US/Engine/Sequencer/HowTo/TriggeringSequences/index.html)

由 Gameplay 来触发 Sequence 的播放。比如如下的情景：
- 当主角走进房屋时，你想要拉近镜头（zoom in），并细看某物品。
- 当主角杀死一个敌人时，想要触发一段影视。

这个教程的主要思路是：
1. 在场景中创建一个 TriggerBox，当主角与 TriggerBox 碰撞时，播放动画序列。
2. 在该动画序列中，可以改变灯光的开闭。
3. 为了使每次碰到 TriggerBox 都播放东湖序列，先 SetPlayBackPositon(0.0)，然后 Play() 这样每次都可以触发。
4. 按 F 键可以快速结束动画序列，可以先检查 Sequencer->IsPlaying()，然后 GoToEndAndStop()，这样既可以结束动画，又可以完成所有的设置。

---

[Blending Gameplay and Sequencer Animation](https://docs.unrealengine.com/en-US/Engine/Sequencer/HowTo/GameplayAnimBlending/index.html)

要将 Gameplay 和 Sequencer 动画混合需要注意如下几点：
1. 将 BP_Character 拖入场景，并且将 Pawn/Auto Possess Player 设置为 Player0，将 Auto Receive Input 设置为 Input0。
2. 这样就可以将场景中的 BP_Character 放入 Sequence 然后添加动画。
3. 在 Sequence 中右键 Animation Track，在 Properties/Slot Name 中可以发现，Sequence 的动画默认在 DefaultSlot。
4. 在动画蓝图中增加一个 DefaultSlot 的 Slot 节点，就可以增加 Sequence 的影响。

因为 Sequence 引起的动画，都通过 DefaultSlot 来输出。当游戏玩法产生的动画，想要和 Sequence 融合。就可以在动画蓝图中使用 Blend节点，Blend(Lococache, Lococache-->Default, BlendInterp)，然后在 Sequence 来 K 这个值。

这个 BlendInterp 可以作为属性，DefaultSlotInterp，并且 ExposeToCinematics，然后定义一个 SetDefaultSlotInterp() 的函数，那么当 Sequence 中发现有该值的帧时，会自动调动该函数。

其实一个 Value 属性和 SetValue() 的函数时匹配的，如果 UE4 识别到 SetValue() 函数，那么设置 Value 时，会自动调用 SetValue()。

---

[Blending Animation Blueprints with Sequencer](https://docs.unrealengine.com/en-US/Engine/Sequencer/HowTo/BlendingAnimBPs/index.html)

Sequence 中的动画和 动画蓝图中的动画可以共存。

Sequence 中的动画，默认是在 DefaultSlot 中，当然在某些特定的情况下，可以设置所在的槽。

Sequence 中的动画，有一个 Weight 的权重混合值，默认是1，如果想要从 Gameplay 的动画逐渐和Sequence融合，可以先从0-1，逐渐从 Gameplay 动画过渡到 Sequence 的动画。

---

[Creating Level Sequences with Dynamic Transforms](https://docs.unrealengine.com/en-US/Engine/Sequencer/HowTo/DynamicTransforms/index.html)

在 LevelSequenceActor 中的 InstanceData 下有两个选项，可以将动画中的位置重新定位。
- OverrideInstanceData，可以重载动画中所有移动的相对 Transform。
- TransformOriginActor，这里可以引用场景中的 Actor，将其用作 Absolute Transform Sections 中的平移远点。
- TransformOrigin，当 Actor 不使用时，可以使用这个设置。

如果这样来做，所有的动画最好都在远点来K，然后在播放时，设置这个标记点，这样很方便就能将动画对其。

这样的话，每一个剧情点，都需要这样一个剧情定位点。【原则】

https://docs.unrealengine.com/en-US/Engine/Sequencer/Workflow/EventTrackOverview/index.html

https://docs.unrealengine.com/en-US/Engine/Sequencer/HowTo/AnimateDynamicObjects/index.html

https://docs.unrealengine.com/en-US/Engine/Sequencer/HowTo/GameplayAnimBlending/index.html
