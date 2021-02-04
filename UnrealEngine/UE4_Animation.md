> 初稿：2020年09月23日

## 官方文档
[UE: Animation](https://docs.unrealengine.com/zh-CN/Engine/Animation/index.html)

## Maya 导入 UE4 的注意事项
1. 窗口--首选项--设置，以便和 UE4 保持一致。
   - 世界坐标系，调整为 Z 轴向上。
   - 工作单位，设置为 厘米。
   - 时间，30fps
2. 将 Maya 时间栏的长度等于动画的长度。
3. 在导出 FBX 时，特别是动画时，选择全部导出，不然只有父级的动画导出，而子级未导出。
4. 将 动画勾选，并且将变形模型勾选。
5. FBX 版本选择 FBX2018，并且上方向为Z。

参考：[Rootmotion动画制作和Mixamo角色动画导入UE4的使用](https://www.bilibili.com/video/BV1Zp411d7E4?t=737)

## 一些技巧
在导入动画时 使用 AnimatedTime 可以只导入有动画的帧。

## RootMotion

RootMotion 启用的步骤
- 将动画的 EnableRootMotion 为真。
- 将动画蓝图的 AnimPreviewEditor\EditDefaults\RootMotionMode 中选择 RootMotionFromEverythin。
- 在 Maya 中添加一个 Root 骨骼。将以前移动的数据，迁移到当前的骨骼上，只迁移移动。在曲线编辑器上，将 Hips 的 平移 x 和 z 复制到 Root 上。然后将 Hips 的值设为0。
  - 但是需要检查第一帧，看是否在 0 点。比如x轴，初始值时 0.4，那么全部选中，输入 `-=0.4` 就可以归零。

在设置 RootMotion 的动画时，应该将移动迁移到 Root 根骨骼上，并最好让开始时为 0。

通过修改动画的 RateScale 来修改 RootMotion 的速度，间接修改了运动的速度。

为了在行为树中也使用移动，需要使用类似转向的功能，而不难直接使用 MoveTo。

使用这个功能，可以用来爬梯子，如果可以给 RootMotion key 动画。

## Montage
B站的 [UE4 Animation Montage (蒙太奇)](https://www.bilibili.com/video/BV1q7411d7xb)，提提供了非常全面的 Montage 的讲述。

在动画蓝图中，使用 Slot 节点来引用，具体每个 Montage 资源的过渡是在各自资源的 BlendOptions 中来设置的。

## 动画通知
[GAS 连击功能](https://historia.co.jp/archives/15422/)
- 使用 NotifyName 来进行区分，ANS_ 的动作窗口，非常有灵性的做法。这种做法只适用于 Montage。
- 使用 Notify State 中的 MontageNotifyWindown 类定义一个输入窗口，可以定义 NotifyName，然后在 PlayMontage 的蓝图中处理 OnNotifyBegin 和 OnNotifyEnd 的事件，然后通过 NotifyName 来进行区分。
- 使用 Notify 中的 MontageNotify，和 MontageNotifyWindow 有类似的功能，然后配个 GAS 系统来处理所有的事情。

在 Anim Notify State 中，比如格斗游戏中，可以在 ReceivedNotifyTick() 中通过曲线来更攻击和防御。
[什么是动画通知状态（Anim Notify State）](https://zhuanlan.zhihu.com/p/339947820)

[UE4音效、动画结合实例](https://www.cnblogs.com/wzj998/p/7512799.html)
- TArray<const struct FAnimNotifyEvent*> AnimNotifies=AnimInstance->NotifyQueue.AnimNotifies;
- 在 C++ 中通过如上的函数，可以获取到所有的动画通知事件，然后查询 NotifyName，这样就可以单独配置了。
- 但是我觉得在动画中配置更好，可以写一个 Python 脚本来快速生成需要要的 Notify。

在动画状态机中可以使用 marker 来显示来对其 Notify 的位置。【问题】


## 动画蓝图之事件图 EventGraph

## 动画蓝图之动画图 Animation Graphs

在 Transition Rule 中，有四个 TimeRemaining 的函数
- GetRelativeAnimTimeRemaining(StateNode)，获取原始状态最相关动画剩下的时间，秒为单位。
- GetRelativeAnimTimeRemainingFraction(StateNode)，也是比例。
- TimeRemaining(AnimName)，获取动画剩下的时间，秒为单位。
- TimeRemaining(Ratio)(AnimName)，获取动画剩下的比率，0-1

基于比率的可能会更好，基于状态可能比基于动画名的更好，因为如果动画改变，这个节点可能就出问题了。

## IK
https://docs.unrealengine.com/en-US/AnimatingObjects/SkeletalMeshAnimation/IKSetups/index.html

[IK节点详解](https://zhuanlan.zhihu.com/p/41425611)

TwoBoneIK，TransformBone 都是在组件空间发生变换的。

用途：
- 将脚贴合到地面
- 将手掌靠着墙
- 将手抓着移动的物体

[用Two Bone IK实现手扶墙](https://zhuanlan.zhihu.com/p/343814763)

[使用Power IK轻松愉快地实现脚底板位置矫正](https://zhuanlan.zhihu.com/p/342095610)

## 笔记
播放和混合已经固定的动画，Animation Sequence
创建自定义移动，比如爬墙和梯子，Anim Montages
产生**伤害效果**和面部表情，Morph Targets。给特定的伤害区域，设置一个 MorphTargets，显示出更精细的控制。
直接控制骨骼，Skeleton Control

BlendPosesbyInt
- 右键，可以添加新的输入针

BlendPoses(Enum)
- 通过右键，可以添加不同的枚举值输入针。

Animation Blend Modes
- 使用 Custom 模式，可以产生不断震荡的转换效果。

## Skeletal Control

Component Space
- Component space assumes the bone's transform to be relative to the SkeletalMeshComponent.
- 是相对于根骨骼进行变换，对于需要调整骨骼位置的，都要相对于根骨骼来变换。

Local Space
- 本地变化是指在当前骨骼的父骨骼空间进行变换。并且最终 Pose 也只接受本地空间的数据。
- Local space assumes the transform of a bone to be relative to its parent bone.

Mesh Space

However, certain blend nodes and all SkeletalControls operate in component-space. 

AnimDynamics
- 提供基于物理的次级动画
- 模拟项链，手镯，包裹，锁链，头发等，一些装饰性物品的动画效果。

Apply a Percentage of Rotation
- 驱动 TargetBone 旋转了 百分之多少的 SourceBone
- 比如驱动角色的弹夹包根据 Spine_01 来旋转

Trail Controller
- 一列动画的影响，比如尾巴

https://www.youtube.com/watch?v=YvXvWa6vbAA
Skeleton Control 的全部演示

AnimationLayer
- 有点类似函数
- 通过 LinkedAnimLayer 来引用动画层。
- 一个状态机中只能有一个，如果要复用，只能通过 CachePose 来做。

我需要对动画状态机中的每一个参数和功能都要熟悉起来。
- PlayAnimationSequence 可以将 Sequence 暴露出来，然后提升为变量，可以通过程序来进行配置。
- 这样就可以对不同的动作进行参数化。比如不同的武器，不同的健康状况，不同的受伤状况，这些参数都可以制作成表格，然后根据状况进行切换。



## 参考
[Making the Most of Animation Blueprints | Dev Days 2018 | Unreal Engine](https://www.youtube.com/watch?v=YjQRBHvltOk)
