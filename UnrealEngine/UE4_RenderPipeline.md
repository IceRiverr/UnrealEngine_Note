> 初稿：2020年07月09日

我需要写我的总结，然后将这个管线中，对我来说比较陌生的地方摘出来，然后一点点的弄明白。先要知道，哪些我懂了，哪些我还有问题，我到底对渲染掌握到了什么程度？

我可以不知道怎么写，但是我需要知道，具体的原理是什么样子的。

## Rendering Pipeline

---

## 参考

[Unreal Frame Breakdown](https://viclw17.github.io/2019/06/20/unreal-frame-breakdown-part-1/)，简洁明了。

[GPU and Rendering Pipelines](https://unrealartoptimization.github.io/book/pipelines/)，
[Unreal’s Rendering Passes](https://unrealartoptimization.github.io/book/profiling/passes/)，[forward-vs-deferred](https://unrealartoptimization.github.io/book/pipelines/forward-vs-deferred/)，从优化的角度来说明UE4管线流程，是优化的一个非常好的参考。

[How Unreal Renders A Frame](https://interplayoflight.wordpress.com/2017/10/25/how-unreal-renders-a-frame/)，一共三篇，是17年的文章。

https://techartaid.com/

[Is Epic doing anything to address UE4's crippling shader problem?](https://forums.unrealengine.com/unreal-engine/feedback-for-epic/78666-is-epic-doing-anything-to-address-ue4-s-crippling-shader-problem/page3?106655-Is-Epic-doing-anything-to-address-UE4-s-crippling-shader-problem=#post866751)

https://viclw17.github.io/archives/#

https://github.com/daozhangXDZ/DaoZhang_ProgramNote/blob/106598d3cc516823c5735503011d8ec3b11e06f0/UnrealStudio/UnrealRender/PipeLine/UE4_PIpeLine.md

http://aicdg.com/ue4-reflection/

https://github.com/daozhangXDZ/DaoZhang_ProgramNote/blob/106598d3cc516823c5735503011d8ec3b11e06f0/UnrealStudio/Animation/Skeletoncontrol_1.md

unrealartoptimization
我通过搜索如上的关键字找到的。

https://github.com/stonelzp/stonelzp.github.io

https://dawnarc.com/ 玄冬Wong

https://zhuanlan.zhihu.com/p/55335907 UE4性能优化

[知乎-Voron：【UE4 Renderer】<04> IBL & SH](https://zhuanlan.zhihu.com/p/47512833)

[知乎-Jiff：UE4 Mobile IBL烘焙实现分析](https://zhuanlan.zhihu.com/p/71513708)

[知乎-YivanLee：虚幻4渲染编程，专题概述及目录](https://zhuanlan.zhihu.com/p/36675543)

延迟渲染，
- 减少多重覆盖光源的性能损失。
- 并不光源能够免费用，知识整个性能更容易预测，和场景中物体的数量无关，而只跟分辨率有关。
- 单一光源的花费只和其覆盖的屏幕面积有关。
- 聚光灯的形状是cone，所以占用的面积小，如无必要，使用聚光灯更省。
- 延迟光照对处理多个小光源很在行。可以给粒子添加光源。

前向渲染
- 可以减少 GPU 内存
- base pass 的费用比较高。

---

#
http://monsho.blog63.fc2.com/blog-entry-193.html
[UE4]尝试各向异性镜面而不扩展GBuffer

#
https://medium.com/@lordned/unreal-engine-4-rendering-overview-part-1-c47f2da65346
https://medium.com/@lordned
Unreal Engine 4 Rendering Part 1: Introduction
--已经读完 OK

# UE4 渲染效果的官方文档
https://docs.unrealengine.com/en-us/Engine/Rendering

https://www.youtube.com/watch?v=zBiIHCuabKw
Custom pass material rendering in UE4

https://forums.unrealengine.com/community/community-content-tools-and-tutorials/17009-tutorial-the-many-uses-of-custom-depth-in-unreal-4
http://www.tomlooman.com/the-many-uses-of-custom-depth-in-unreal-4/
[Tutorial] The many uses of Custom Depth in Unreal 4

http://blog.felixkate.net/2016/05/22/adding-a-custom-shading-model-1/

https://www.raywenderlich.com/57-unreal-engine-4-custom-shaders-tutorial
Unreal Engine 4 Custom Shaders Tutorial
这个网站的游戏教程非常有趣

https://www.youtube.com/user/VirtusEdu/

https://www.youtube.com/channel/UCN0ltBbl0xwZeYg6hNlXrvA
游戏教程很好

Gnomon Masterclass Part I: Building Better Pipelines for UE4 | Event Coverage | Unreal Engine
https://www.youtube.com/watch?v=vxyjE1gBcZw&t=3500s

Gnomon Masterclass Part II: Rendering in UE4 | Event Coverage | Unreal Engine
https://www.youtube.com/watch?v=kp3zcyZZBVY、
看了CPU端的Culling

Unreal Engine by Epic Games: Lighting and Rendering
https://www.youtube.com/watch?v=qu3NfoXNZG4

UE4 MasterClass
https://www.youtube.com/watch?v=hcxetY8g_fs&list=PLZlv_N0_O1gbRdp_2fmNxPCxPaSIsBSX5

Lighting with Unreal Engine Masterclass | Unreal Dev Day Montreal 2017 | Unreal Engine
https://www.youtube.com/watch?v=ihg4uirMcec
* Light Baking

Unreal Studio - Lightmass and Beyond
https://www.youtube.com/watch?v=TzqpyNb0998

https://www.youtube.com/watch?v=-sJsR0y7R8U&list=PLzGMJVlc_bxW1Sr0PJg68nRqIcFG52Erj
UE4 Light 光照计算
Baked Lighting & Lightmaps

Unreal Engine 4 Lighting Masterclass (Summary)
https://www.tomlooman.com/lighting-with-unreal-engine-jerome/

UE4 - How to use Lightmaps - Rules and Guidelines
https://www.youtube.com/watch?v=gMXye-JhCyQ&list=PLzGMJVlc_bxW1Sr0PJg68nRqIcFG52Erj&index=2



UE4 渲染系统学习过程
1 DepthPass 和 BasePass
2 PBR ShaderModel，以及材质的编译过程
3 DeferredLightCalc 延迟渲染技术
4 GI的实现方法

RenderTargets
https://www.youtube.com/watch?v=1Z-V1Buk6z8

https://www.youtube.com/watch?v=QGIKrD7uHu8
Content-Driven Multipass Rendering in UE4 | GDC 2017 | Unreal Engine
https://shaderbits.com/blog/getting-the-most-ouf-of-noise-in-ue4
https://www.pinterest.jp/pin/467881848773625294/?lp=true


---

[UE4 4.23 移动端渲染的Dynamic Instancing分析（上）](https://www.youtube.com/watch?v=qx1c190aGhs&t=1219s)

[UE4渲染框架解析之InitViews上篇](https://zhuanlan.zhihu.com/p/81308446)

腾讯渲染引擎，那本书的架构仿写了大量UE4的内容，可以作为自己理解UE4一个起步。

---

## Mesh Drawing Pipeline

https://godofpen.blogspot.com/2019/04/take-look-at-mesh-drawing-pipeline-in.html

https://docs.unrealengine.com/en-US/Programming/Rendering/MeshDrawingPipeline/index.html

[UE4.22渲染数据管线重构和动态instancing](https://zhuanlan.zhihu.com/p/73580221)

[基于UE4的 Mobile Skeletal Instance——MeshDrawCommand](https://zhuanlan.zhihu.com/p/158073773)

[Unreal 4.22渲染数据管线重构和动态Instancing](https://zhuanlan.zhihu.com/p/75352168)

[虚幻4渲染编程(Shader篇)【第十二卷：MeshDrawPipline】](https://zhuanlan.zhihu.com/p/61464613)

