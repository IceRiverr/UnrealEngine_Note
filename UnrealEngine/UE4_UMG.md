> 初稿：2020年10月29日

总结整理 UE4 UI 的功能。

---

## 技巧

可以重载 OnPaint 来 DrawLine。

## UI Animation

可以给 UI 元素key动画，然后产生渐变、平移、旋转等不同的特效。

[UI ANIMATION: Everything You Need to Know | UE4 Tutorial](https://www.youtube.com/watch?v=EK0di47pdY0)

## ListView
[知乎：UMG中新手必晕的ListView详解](https://zhuanlan.zhihu.com/p/127184008)

## OnKeyDown
有些情况下 OnKeyDown 不起作用，如果 Widget 中没有 Focuse 的控件。
- KeyBoard Focus
- SetInputModeUIOnly
- 

## 参考

https://docs.unrealengine.com/en-US/Engine/UMG/index.html

[第三篇：UE4如何制作3DUI，使3D UI跟随摄像机旋转](https://blog.csdn.net/Sxx930923/article/details/83089318)

实现思路：
1. 创建一个 Widget 的 UI，比如 UI_Bullet。
2. 自定以一个 Actor BP_Gun，挂上 UI_Bullet 的 Widget 组件。
3. 在 BP_Gun 的 Tick() 中，更新 UI 的方向。
   1. CurrentRotation = Self->GetActorRotation()
   2. TargetRotation = FindLookAtRotation(Self->GetActorLocation(), GetPlayerCameraManager()->GetActorLocation)
   3. TempRotation = RInterpTo(CurrentRotation, TargetRotation, dt)。
   4. Self->SetActorRotation(TempRotation)

[UE4 UMG优化](https://zhuanlan.zhihu.com/p/150118384?from_voters_page=true)

[UE4入门之路（UI篇）：UMG系统介绍](https://zhuanlan.zhihu.com/p/117582084)，非常详细。

[UE4入门之路（UI篇）：3DUI的制作](https://zhuanlan.zhihu.com/p/117564146)

[UE4入门之路（基础蓝图篇）：蓝图的制作](https://zhuanlan.zhihu.com/p/117591827)