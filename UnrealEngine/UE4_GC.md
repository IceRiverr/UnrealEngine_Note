> 初稿：2020年12月21日

[UE4GarbageCollection.md](https://gist.github.com/LordNed/650de717daca23aaf8fc2932bf3f9123)
GC 是一种自动内存管理机制。UE4 在 UObject 中实现了这一套系统。所有继承自 Uobject 的类的对象，都会进入 GC 系统。
UE4 保存有一组 Root Set 对象，每次 GC 都会遍历，然后需要被清楚的对象，在这个引用关系中找不到，就会被清除。

https://docs.unrealengine.com/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/Objects/Optimizations/index.html
自动更新 UProperty 的机制只适用于 Actor、Component 和 UE4 的集合类。

[知乎：对象内存管理几种模式](https://zhuanlan.zhihu.com/p/49046671)

