> 初稿：2020年09月14日

## 我的总结
UE4 在 创建内容时，可以创建蓝图类和资源，我以前把这两项混淆了。

[UGameplayStatics](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Kismet/UGameplayStatics/index.html)，提供了游戏机制的大量使用函数，并且都是静态函数。非常好用。

Object->Actor+Component->Level->World->WorldContext->GameInstance->Engine

和 World 有关的数据都派生字 Actor，比如 AGameMode，AGameState，AWorldSettings，AController，APlayerState

---

[(UE4 4.20)UE4 FSoftObjectPath,FSoftClassPath,FSoftObjectPtr,TSoftObjectPtr,TSoftClassPtr,TSubclassOf](https://blog.csdn.net/qq_29523119/article/details/84929384)

[(UE4 4.20)UE4同步加载和异步加载UObject ----------LoadObject,LoadClass,FStreamableManager](https://blog.csdn.net/qq_29523119/article/details/84455486)

---

## UE4 Gameplay Framework
http://www.tomlooman.com/ue4-gameplay-framework/

在 Actor::Tick 尽量少些代码，尽量使用事件驱动的方式来进行开发。

我可以画一些图，来图示一些功能。

---

## Game jam tips and tricks
[Game jam参赛技巧 | Game jam tips and tricks](https://www.bilibili.com/video/BV1zK4y1x7gH)

提供了大量非常有趣的工具和 Gameplay 的实现方法。比如射击时，Spawn 一个 Actor，使用这个 Actor 作为容器来存储各种 Effect，然后在 BeginPlay 中调用计算。

