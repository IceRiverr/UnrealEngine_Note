> 初稿：2020年11月28日

将和 UE4 有关的一些疑问和思考，主要是一些疑问，先记录在这里，以防忘记。

---

## UE4 的蓝图类和C++类到底有什么区别？【问题】
- 其在实现层级是怎么实现的？在底层的结构？【问题】
- what is the difference between blueprint class and c++ in ue4
- https://stackoverflow.com/questions/38103176/are-ue4-blueprints-the-same-with-a-c-class-if-so-how-will-i-implement-a-clas
- 可以编译成 C++ 类。

https://docs.unrealengine.com/en-US/Engine/Blueprints/UserGuide/Types/ClassBlueprint/index.html

蓝图类和具体的资源有什么区别？将蓝图类作为资源来使用，或者资源的配置，是否合适？

要解决这个问题，需要去看代码。

https://docs.unrealengine.com/en-US/Engine/Blueprints/TechnicalGuide/index.html

Blueprint 的 Construction Script，可以用来作为程序化生成，可以当做一个 prefab 系统。

比如我又一个英雄角色，有很多的成员变量。那么我仅仅想要改变其 SkeletonMesh 就生成一个子蓝图，是一个当下必然的选择。但是我还是有点担心性能问题。

## UE4 如何制作 Mod 和 DLC？

## UE4 的 Datatable 中引用资源和路径有什么区别？

## Name String Text
FName，给某个东西命名的，FName不区分大小写，内部转化为哈希，比较更加方便。
FText，被显示的字符串，可以用于多语言功能，支持内置本地化支持。【待做】
FString，类似 std::string