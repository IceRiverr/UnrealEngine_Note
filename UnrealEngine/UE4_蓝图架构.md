> 初稿：2020年12月14日

现在时间是晚上10点，我准备细心地看一部分技术文档，并且为自己的目标而前进。

## 调试

在 DefaultEngine.ini，增加一下两行：

``` ini
[Kismet]
CompileDisplaysBinaryBackend=true
```

就可以在OutputLog窗口里看到编译出的字节码。

## 笔记
每一个蓝图，在保存时，就生成了一个 BP.uasset 的文件。本质上是一个 UObject 序列化后的结果。

在整个UObject及其子对象组成的树状结构中，只有最外层（Outermost）的对象是同一个对象时，才会被序列化到一个.uasset文件中

蓝图编辑器中的BeginPlay事件节点对应的并不是AActor::BeginPlay()，而是AActor::ReceiveBeginPlay()这个事件。

```c++
/** Event when play begins for this actor. */
UFUNCTION(BlueprintImplementableEvent, meta=(DisplayName = "BeginPlay"))
void ReceiveBeginPlay();
```

`ReceiveBeginPlay()` 是“蓝图实现的事件”。只是看上去叫 `BeginPlay()` 。

```c++
static FName NAME_AActor_ReceiveBeginPlay = FName(TEXT("ReceiveBeginPlay"));
void AActor::ReceiveBeginPlay()
{
    ProcessEvent(FindFunctionChecked(NAME_AActor_ReceiveBeginPlay),NULL);
}
```

为什么 FName 的申明使用的是，FName(TEXT("BeginPlay"))。【问题】

```c++
 /** Opportunity for blueprints to handle the game instance being initialized. */
 UFUNCTION(BlueprintImplementableEvent, meta=(DisplayName = "Init"))
 void ReceiveInit();

void UGameInstance::Init()
{
    ReceiveInit();
    ...
}




```

## 参考
https://docs.unrealengine.com/en-US/ProgrammingAndScripting/Blueprints/TechnicalGuide/Compiler/index.html

[房燕良：理解蓝图技术架构](https://zhuanlan.zhihu.com/p/92268112)

[虚幻4蓝图虚拟机剖析](https://www.cnblogs.com/ghl_carmack/p/6060383.html)

https://unktomi.github.io/Latent-Actions-Cont/Cont.html

https://answers.unrealengine.com/questions/934656/bp-virtual-machine-and-threads.html

https://www.casualdistractiongames.com/post/2016/05/15/creating-latent-functions-for-blueprints-in-c