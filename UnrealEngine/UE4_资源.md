> 初稿：2020年07月28日

## 渲染管线



## 2019年12月04日 渲染与UE4资源整理
将知乎上和UE4有关的资料都整理出来，包括CSDN，以及博客园，以及B站，以及油管上官方的资料。

UE4官方的文档和教学视频

将和Unity3D有关的资料也整理出来。以及想要做的部分游戏都做一些整理。

将渲染相关的学习整理路径，都整理出来，然后指定相应的学习计划。
需要看的书，教程，需要写的代码，如何搭建测试框架等，都确定好。
* 图形学的一部分书，其实整个的框架已经是对的
* OpenGL的框架
* DirectX的框架
* 一部分网上博客
* 一部分网上的公开课
* 已经渲染引擎的结构系统
* Realtime Rendering 4

将需要看的主题都一一列举出来，直到能够完全弄清楚为止。
目前确定下来的一个主题是关于光源的。

英语方面的预计时间要定下来。

关于渲染系统方面的思考可能要手机部分资料，并且配合UE4渲染部分的代码一起来学。
通过分解UE4的渲染系统，用来理解更通用的渲染理论和工程实践。
其的整个渲染流程，优化策略，以及管线安排，场景管理等，都去分析清楚。

现在我来总结一下知乎上UE4相关的内容都有些什么。


Voron，一共写了6篇和UE4渲染底层知识有关的文章
https://zhuanlan.zhihu.com/p/39464715 【UE4 Renderer】<03> PipelineBase
* 这篇一定要好好看看，关于管线的部分讲的非常详细

Jerry，腾讯客户端
https://zhuanlan.zhihu.com/p/57561144 UE4渲染模块概述（一）---方法论、遮挡剔除、Geometry Rendering
* 他对渲染模块和物理模块都有分析，并且分析的很宏观，或者说非常有高度，可以优先来看
* 实际上是对 Gnomon Masterclass Part II:  | Event Coverage | Unreal Engine 的总结

Jett Huang，他的分析非常细，包括对象系统和渲染，物理
https://www.jianshu.com/p/4d1be6c2b4ef UE4渲染模块分析
* 这个分析还是比较细的，可以整体看一看
* 场景的描述
* 场景遍历和拣选
* 渲染的执行
* 主要是代码分析

大钊 InsideUE4，UE4社区经理
https://zhuanlan.zhihu.com/p/22813908
* GamePlay 架构
* UObject 类型系统
开始看UObject的类型系统，在2019年12月06日这个周末应该可以看完。


关于虚幻的UBT系统（Unreal Header Tool），需要好好分析，不要落下。


YivanLee UE渲染编程，腾讯技美
https://zhuanlan.zhihu.com/p/36675543
https://blog.csdn.net/qq_16756235/article/list/1 魁梧的抠脚大汉
* 材质编辑器
* 特效
* Shader的修改和定制
* Ray Marching
* Multi Pass Rendering
* 图元汇编
* Tone Mapping
* 自定义管线
* 物理模拟
* 动画，自定义动画节点
* Slate UI界面
* ## 是学习整体UE4渲染最有价值的地方。


MoonChildInSky GT虚幻引擎笔记目录
https://zhuanlan.zhihu.com/p/41323691
https://blog.csdn.net/DoomGT/article/details/79844191
* 关卡的制作流程，以及注意事项，以及部分译作
* 特殊的材质氛围的调整


人宅 UE4插件与编辑器Slate，用极短的时间学会，学会游戏开发的技术
https://zhuanlan.zhihu.com/p/53659988
https://www.aboutcg.org/courseDetails/470/introduce
* 在知乎的文章中，有一些前导的学习路径
* 价格258，时间7小时
* 主要是UMG和Slate的架构，对这个体系进行深入

https://zhuanlan.zhihu.com/p/60117613
* 人宅的UE4教程简谱

https://www.aboutcg.org/teacher/4202 AboutCG 上其教学视频地址
* 人宅的其他的教程
* 编辑器开发，多线程，独立游戏C++使用，多线程，蓝图
* 这是一个面想如何自己制作游戏的角度，来进行知识体系搭建的
* 其专栏中有部分基础知识点的总结

https://zhuanlan.zhihu.com/p/71704642
* UE4底层入门三部曲
* 插件+编辑器+蓝图，反射系统


https://zhuanlan.zhihu.com/p/62373752 Unreal编辑器拓展_Gizmo

UE4开发套路1-5，
https://zhuanlan.zhihu.com/BugMarker 工作中的UE4
https://zhuanlan.zhihu.com/p/29285794
* 作者是UE4程序
*  这个系列主要是怎么对UE4进行扩展，以及日常工作的需求怎么来实现，提供了思路


Solar Ant，美工， UE4渲染流程源码分析
https://zhuanlan.zhihu.com/p/73052457 虚幻4笔记-引擎启动和核心循环源码分析
https://zhuanlan.zhihu.com/p/73381385 虚幻4笔记-渲染线程源码和TaskGraph多线程机制源码实现分析
https://zhuanlan.zhihu.com/p/73586731 UE4笔记-渲染流程源码分析(引擎Tick开始)
https://zhuanlan.zhihu.com/p/74246048 UE4笔记-渲染流程源码分析之静态网格创建到渲染命令创建
* 该作者对渲染代码的分析有三篇，可以作为自己看代码的一个辅助。

https://zhuanlan.zhihu.com/p/47512833 【UE4 Renderer】<04> IBL & SH
* 包括 光栅化，Slate系统，PipelienBase，IBL&SH，VoxelGI，TMap


Maga Anti，UE4渲染框架解析
https://zhuanlan.zhihu.com/p/80826700 UE4渲染框架解析之数据更新
* 主要是面向代码的，可以看看
* 阴影，InitVIews，渲染线程同步


张鑫，UWA
https://zhuanlan.zhihu.com/p/58963258 UE4的HISM深入探究
https://blog.uwa4d.com/archives/USparkle_UnrealEngine.html Unreal 4.22渲染数据管线重构和动态Instancing
https://zhuanlan.zhihu.com/p/31117091 虚幻引擎学习之路：渲染模块之材质系统
* 从材质制作和使用的角度来认识UE4的渲染
* 虚幻引擎学习之路的整个系列可以用来作为UE4学习的帮助
* 整个UWA的“学习之路”系列是非常好的学习整个引擎的资源。到 https://blog.uwa4d.com/search/%E8%99%9A%E5%B9%BB/


More ever
https://zhuanlan.zhihu.com/p/36027034 UE4 StaticMesh渲染排序
* 这个人的部分渲染分析可以看


https://zhuanlan.zhihu.com/p/32406413 UE4 渲染基础 RenderTranslucency

https://zhuanlan.zhihu.com/p/80804392 B站的教学视频总结

Jiff
https://zhuanlan.zhihu.com/p/72086470 UE4渲染引擎导读（序）
* 解析LIghtmass
* 解析材质系统
* PBR解析


https://www.youtube.com/watch?v=kp3zcyZZBVY&t=1762s Gnomon Masterclass Part II:  | Event Coverage | Unreal Engine
* 实际上是对这个视频的总结
别人的总结
https://www.cnblogs.com/SeekHit/p/8492364.html Rendering in UE4
https://zhuanlan.zhihu.com/p/35075351 Rendering in UE4（Epic Game TA-Homam Bahnassi讲座个人笔记）

https://zhuanlan.zhihu.com/p/60985045 人人都能看懂的UE4光追（GDC 2019）

https://docs.unrealengine.com/en-US/Engine/Rendering/VisibilityCulling/index.html



Dragon Sama，腾讯客户端
https://www.zhihu.com/people/dragonsama/posts
https://zhuanlan.zhihu.com/p/34473064 UE4中的基于物理的着色（一）
* 关于UE4的物理着色的总结
* 以及资源管理


https://zhuanlan.zhihu.com/p/43742565 Epic Games 王祢：UE4制作多人大地型游戏的优化

https://zhuanlan.zhihu.com/p/47075752 UE4 渲染部分1：介绍
* 对老的UE4整个渲染系统的解析

https://zhuanlan.zhihu.com/p/33865743 译：UE4是如何渲染一帧的
* 主要从宏观的角度来分析渲染流程和管线，通过RenderDoc


https://interplayoflight.wordpress.com/2017/10/25/how-unreal-renders-a-frame/
https://medium.com/@lordned/unreal-engine-4-rendering-overview-part-1-c47f2da65346

罗传月武
https://zhuanlan.zhihu.com/p/88013269 如何利用虚幻商城创造被动收入【经验分享】

https://zhuanlan.zhihu.com/p/83385455 虚幻引擎[真实字幕组]开始公开招募！这太真实了！

https://zhuanlan.zhihu.com/p/95146912 UE4 升级一个卡通渲染模型到4.23版本

https://zhuanlan.zhihu.com/p/78174277 UE4 级联阴影（CSM）和逐物件阴影
* 这个文档写的非常仔细，需要好好来看


春日，虚拟现实的教师，有大量面部捕捉方面的总结
https://zhuanlan.zhihu.com/p/57447319 【03】UE4渲染管线研究 以及SSSS实现 第一部分
https://zhuanlan.zhihu.com/p/59068946 【04】UE4 的 SSSS 渲染管线 II

https://zhuanlan.zhihu.com/p/33824889 Epic Game TA-Homam Bahnassi讲座个人笔记 《UE4中的渲染》

— 好了，准备睡觉了，整理的头有点涨，现在时间是 2019年12月05日01:54:53，以后不敢这样了。

https://zhuanlan.zhihu.com/p/94332942 虚幻周报20191124 | 巨多资料必看！

Unreal Cook Book

https://blog.csdn.net/justtestlike/article/details/84941262  自定义资源_Custom Assets_UE4_C++
https://gameinstitute.qq.com/community/detail/122090 UE4动画系统概述

https://gmpreussner.com/reference/adding-new-asset-types-to-ue4 
* 添加一个 Text Asset

---

## 2019年12月10日 UE4反射与蓝图

晚上在旁边的公园读了会英语，将今天早上欠的英语朗读补起来了。刚刚将英文的功课做完了，大约到10：40左右。

今天看了一天UE4的反射和蓝图系统，看得有点晕，需要重新来看一遍。
需要将今天看过的东西好好整理一下。

反射系统
https://cloud.tencent.com/developer/article/1176520 我所理解的C++反射机制

https://preshing.com/20171218/how-to-write-your-own-cpp-game-engine/
https://preshing.com/20180116/a-primitive-reflection-system-in-cpp-part-1/ 
https://preshing.com/20180124/a-flexible-reflection-system-in-cpp-part-2/
* 反射实现，其实现了一个方便的反射系统，用来进行资源流水线，以及渲染

https://zhuanlan.zhihu.com/p/24319968 Inside UE4 UObject（1）开篇
* 大钊的Uobject完整解析。
* 非常经典

https://www.bilibili.com/read/cv1568720/ UE4 反射机制使用(进阶文章)

http://aicdg.com/ue4-reflection/ UE4的反射系统

https://www.cnblogs.com/ghl_carmack/p/5701862.html 深入研究虚幻4反射系统实现原理（一）
* 着是大钊之前非常有名的一个分析，
* 版本比较旧

https://zhuanlan.zhihu.com/p/46836554 UE4中的反射之一：编译阶段
https://zhuanlan.zhihu.com/p/46837239 UE4中的反射之二：运行阶段
* 这个看的很不错

https://zhuanlan.zhihu.com/p/54391208 虚幻4反射系统（一）
https://zhuanlan.zhihu.com/p/54392428 虚幻4反射系统（二）
* fengliancanxue 这个不错

https://zhuanlan.zhihu.com/p/75526405 UE4 UObject系列序
* 一个程序猿的分析，没有看过，待看

https://www.cnblogs.com/ghl_carmack/p/5698438.html 虚幻4属性系统（反射）翻译

https://ke.qq.com/course/442518?taid=3638949696684182
* 这是一个视频，第一个视频可以看
* 序列化，方便保存和网络传输
* 引用计数
* 垃圾回收

https://zhuanlan.zhihu.com/p/28795368 UE4对象系统_序列化和uasset文件格式
https://zhuanlan.zhihu.com/p/28661691 UE4对象系统_UObject&UClass
* JettHuang 在简书上做的部分总结，时间比较旧，比较散

https://zhuanlan.zhihu.com/p/24469028 UE4 对象类型Class及内存管理（1）
https://zhuanlan.zhihu.com/p/24471743 UE4 对象类型及内存管理（2）

https://zhuanlan.zhihu.com/p/81319106 C++学习，UE4的反射学习（暂时···以后持续更新）

蓝图系统
https://zhuanlan.zhihu.com/p/54174686 虚幻4蓝图虚拟机剖析
* 这个写的最好

https://www.jianshu.com/p/4cffb2349c22 UE4对象系统_UFunction
https://www.jianshu.com/p/2e2e70730abf UE4对象系统_UFunction_字节码执行
* 接下来这两个写的非常好，将字节码的逻辑都讲明白了
* 并且他的其他Object分析也分厂帮

https://zhuanlan.zhihu.com/p/69067129 UE4蓝图解析（一）
这一篇，一共写了四篇，分析了蓝图的节点是如何一步步转化成字节码的，非常有价值。

https://www.cnblogs.com/ghl_carmack/tag/ue4/ 
https://zhuanlan.zhihu.com/p/54486088 虚幻4蓝图编译剖析（一）
https://zhuanlan.zhihu.com/p/54486475 虚幻4蓝图编译剖析（二）
* 讲到字节码的部分，以及蓝图生成的字节码

https://www.iteye.com/blog/rednaxelafx-492667 虚拟机随谈（一）：解释器，树遍历解释器，基于栈与基于寄存器，大杂烩
* 纯理论

https://andreabergia.com/tags/stack-based-virtual-machines/ 实现教程
* 使用Java实现的

UE4 其他知识
https://peterleontev.com/blog/unreal_cast/ HOW UNREAL ENGINE C++ CAST<T> FUNCTION WORKS?
https://peterleontev.com/blog/level_streaming_optimization/ LEVEL STREAMING AND GARBAGE COLLECTION OPTIMIZATION TWEAKS IN UNREAL ENGINE 4

https://www.bilibili.com/read/cv3075822 最美马赛克——简析八方旅人的render path
https://www.bilibili.com/read/cv2715877 Unity LWRP vs HDRP

https://www.bilibili.com/read/cv1480635 [译]How GPU works

https://wiki.unrealengine.com/index.php?title=Garbage_Collection_%26_Dynamic_Memory_Allocation

https://gameinstitute.qq.com/community/detail/101085 Introduction to C++ Programming in UE4

https://www.aboutcg.org/courseDetails/561/introduce 人宅  反射系统 的教程

https://zhuanlan.zhihu.com/p/84491200 UE4关于PBR工作流程的学习笔记



https://zhuanlan.zhihu.com/p/77784922 浅析创建自定义资源

---
https://zhuanlan.zhihu.com/p/36027034 UE4 StaticMesh渲染排序

[Mage Anti：UE4 图形学解析](https://zhuanlan.zhihu.com/p/80522449)

