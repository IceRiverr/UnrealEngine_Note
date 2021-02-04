> 初稿：2020年09月15日

先了解功能，具体实现了什么功能，然后再去了解细节。
对于底层，就需要一点一点的啃，要做到透彻理解。
对于使用，要像使用工具一样，做到熟练，从熟练中生出神来，做到游刃有余。
而 UObject 适合一点一点的啃。

在写作中，将“主谓宾”三者都尽量明确。

---

## UE4 UObject 的基本事实

UObject 中有大量的接口，继承链：
UObject--UObjectBaseUtility--UObjectBase。

sizeof(UObject) == 56。
光一个 UObject 需要56 个byte。
1万个对象是546 k，1百万个对象是 53M。

UE4 提供了大量 UClass，NewObject，UHT 等辅助类。

UObject 这样一个统一的基类，为引擎中其他的功能实现提供了便利。

## UObject Outer
https://answers.unrealengine.com/questions/292594/how-can-i-understand-the-data-member-outer-in-the.html

一个 UObject 对象的 Outer，是说谁拥有这个对象，谁就是 Outer。
比如 Componennt-->Actor-->Level-->World，这样逐级上升，往往上层是底层对象的 Outer。

`template<class TReturnType> TReturnType* CreateDefauSubobject(FName SubobjectName, bool bTransisent = false)`

`CharacterMovement = CreateDefauSubobject<UCharacterMovementComponent>(Name);`

这个函数在内部调用是，将当前的 Actor 当做是 Outer。

https://docs.unrealengine.com/en-US/Support/Builds/ReleaseNotes/2015/4_9/index.html
New: Construct Custom Objects in Blueprints
> The "Outer" input will serve as the new object's owner, which controls the lifetime of the created object.

> For actor classes, you'll still use the Spawn Actor From Class node. And for widgets, you'll use the Create Widget node. In a future release we may try to combine these different features.

`world->SpawnActor<T>()` 时，其内部会调用 `NewObject<T>()` 其中的 Outer 是 Level。

## 我的笔记
[UE4_UObject之大钊的博客笔记](./UE4_UObject之大钊的博客笔记.md)


## 参考
[知乎-独傲雕：UE4 UObject系列序](https://zhuanlan.zhihu.com/p/75526405)


[凡事看本质：UE4入门与精通](https://www.cnblogs.com/ghl_carmack/p/5677090.html)

