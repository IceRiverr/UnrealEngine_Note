> 初稿：2021年1月1日

## 核心概念
> For those who are not familiar with the system, in a regular GAS pipeline, there are four main classes: the Ability System Component that attaches to the owner, the Attribute Set that is defined as the owner's property, the Gameplay Effect that could be applied to the owner of the Ability System Component and modifies attributes defined in an Attribute Set, and finally, there is the Ability, the action controlled by Gameplay Tags and Effects. There are also optional classes like Gameplay Task (which are sub objects of Abilities) and Gameplay Cues that help provide visual feedback from Gameplay Effects.
-- [Unreal’s Gameplay Ability System is at the heart of Atomic Heart](https://www.unrealengine.com/en-US/tech-blog/unreal-s-gameplay-ability-system-is-at-the-heart-of-atomic-heart?sessionInvalidated=true)

UAbilitySystemComponent - GSC 技能系统组件
- 只有包含该组件的 Actor 才能释放技能。
- 负责管理其他的技能系统

UGameplayAbility - GA 技能逻辑，游戏的能力和技能
- 主动技能，被动技能
- 受击、格挡、击退
- 定义技能逻辑，技能的主题逻辑就写在这里
- 不同的 Actor 上学习、取消、释放不同能力构成了主体逻辑
- 技能的释放，被 Gameplay Tags 和 Effects
- 控制技能的逻辑写在 UAbilityTask 中。
- 技能有 Started，InProgress，Stopped 等概念。
- CommitAbility，用来计算这个技能的 cost，然后决定是否执行。会执行对应的Effect，比如扣血，消耗攻击点。

UAbilityTask
- 其不止是一个函数，同时是一个蓝图节点。即 UObject
- 每一个 Task，会产生一个实例，放到 GameplayAbilityComponent 中。
- 有大量 RootMotion 节点，为了匹配战斗系统中根运动的需求
- Root Bone 上的数据会被添加到 CharacterMovementCompponent 上。【待定】
  - Root Moiton System

UGameplayEffect - GE 技能效果，影响和作用。
- 属性的修改和动作效果触发
- 对 Attribute 的改变都通过 GE 来改变
- GE_HealthPickup, GE_DamagePickup
- 每一个 Effect 都需要有 Target，有些需要有 Owner，这才是一个

UGameplayCueNotify - GC 技能特效
- 爆炸
- 持续性的特效，火的燃烧
- hit back
- UGameplayCueNotify_Static is a one shot effect.
- UGameplayCueNotify_Actor，是一个 Actor，可以写更复杂的功能。
- Spawn Emitter，Fire and forget
- 是一个很通用的 Companion class（伙伴类），和 GE 搭配使用

FGameplayAttribute - Attribute 游戏属性
- 生命力，攻击力，防御力
- 多个 Attribute 可以组合成 AttributeSet，挂在 Actor 上。

FGameplayTag - Tag 标签
- 层次化的判断和搜索不同的条件
- 用好了 Tag，基本就理解了 GAS 的精髓。

FGameplayTask - Task
- 异步操作
- 比如 Montage 播放结束后，再结束技能

FGameplayEventData - Event
- ASC 之间，可以发送一些时间来通知对方

GameplayTaskComponent 可以调用大量的异步技能。

## 参考
[UE4 GameplayAbilitySystem](https://docs.unrealengine.com/en-US/InteractiveExperiences/GameplayAbilitySystem/index.html)

[GAS 连击功能](https://historia.co.jp/archives/15422/)
- 使用 NotifyName 来进行区分，ANS_ 的动作窗口，非常有灵性的做法。
- 使用 Notify State 中的 MontageNotifyWindown 类定义一个输入窗口，可以定义 NotifyName，然后在 PlayMontage 的蓝图中处理 OnNotifyBegin 和 OnNotifyEnd 的事件，然后通过 NotifyName 来进行区分。
- 使用 Notify 中的 MontageNotify，和 MontageNotifyWindow 有类似的功能，然后配个 GAS 系统来处理所有的事情。
- 这两个文档，我需要重新看一遍。

[GAS 耐力系统](https://historia.co.jp/archives/17941/)
- 创建了一个非常灵活的耐力值的消耗系统。

这两个博客的内容，今天晚上面试完，在自己的电脑上复现一下。

[Action RPG: Gameplay Abilities System | Inside Unreal](https://www.youtube.com/watch?v=QZk3tEpZxjU)

[A Guided Tour of Gameplay Abilities | Inside Unreal](https://www.youtube.com/watch?v=YvXvWa6vbAA)
- knockback
- 他的战斗系统，是基于玩家的位置来摆放的。

[Unreal’s Gameplay Ability System is at the heart of Atomic Heart](https://www.unrealengine.com/en-US/tech-blog/unreal-s-gameplay-ability-system-is-at-the-heart-of-atomic-heart?sessionInvalidated=true)
- 在几周之内，进行系统的培训。
- Cue 和 Effect，通过 Tag 来通信。这样管理的任务就留到了 Tag 的管理和配置上。但是 Effect 和 Cue 的耦合就从代码中解绑了。

[GASStreamAbilityDemo.zip](https://epicgames.ent.box.com/s/j7kyfvcflgem7mfmwc237m1va7cl463j)

把大钊对这个系统的讲解梳理一遍。

使用范例来进行理解和记忆。

如果要学习的话，需要将 ARPG 中的每一个技能流程都看一遍，最好自己配置一遍。然后彻底掌握。