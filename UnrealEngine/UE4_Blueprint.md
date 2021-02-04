> 初稿：2020年09月30日

## 蓝图编程

Latent Function
- Delay
- Timer
- 以及一些 Completed 的函数。

这样就就可以再线性逻辑中写一个跨时间的一些函数。我刚开始写的时候，有一些不适应。

## 蓝图插件
有一些辅助的蓝图插件，比如整理蓝图节点，使其更加紧凑。有空找一找。【待定】

## 蓝图路有点
可以通过蓝图路有点来优化蓝图网络，使节点网络更加清晰。因为通过这些路有点可以人为控制网络的路径。

## 蓝图函数库
遇到新的函数时，到 UE4 的官方文档看一下说明，慢慢积累。[Doc](https://docs.unrealengine.com/en-US/index.html)。

UDataTableFunctionLibrary
- void GetDataTableRowNames(UDataTable * Table,TArray < FName >& OutRowNames)
- bool GetDataTableRowFromName(UDataTable * Table, FName RowName, FTableRowBase & OutRow)

这是数据表函数库中最常用的函数。

UGameplayStatics
- SpawnEmitterAtLocation()，Fire And Forget，即发即忘

## 其他对象

Actor
- GetComponentByClass() 


## 蓝图节点

流程控制
- Branch，类似于 if。按住 B 键，鼠标点击就可以生成一个分支节点。`if(var){}else{}` 使用变量作为判断条件。
- DoOnce，只执行一次，可以在 Reset 来连接一个执行线。如果 Reset 执行了，那么就可以继续执行一次。
- DoN，可以设置最多可以执行 N 次，同样可以将内部的计数器重置。
- DoOnce MultiInput，可以对多个输入只执行一次。
- Flip Flop，对内部的 bool 开关进行不断反转，默认首先是 True。然后首先走 A 输出。
- Gate，包含有 Open，Close，Toggle 来控制这个门逻辑的开闭。这些都可以通过输入事件来确定。
- 参考：[UE4流程控制](https://www.jianshu.com/p/dd681a3dbde1)

#

Split Struct Pin
- SetActorRotation() 时，点选Rotation，Split Struct Pin，可以将 Rotation 分裂为 x-y-z 三个输入，方便使用。


Break
- Break HitResult


Make
- Make Array，建立一个数组


Restore all Struct pins
- 在设置 Array 中的成员时，通过右键该节点，将结构体打散，然后挨个进行设置。


RemoveAllOtherPins
- 将 Array 设置中的别的输入隐藏掉。


Division
- 可以获取商和余数


ForEachLoop
- ArrayElement 是一个临时变量，而不是引用，需要注意。


Map
- 图 Map 中，一旦生成键值对，值就不能修改了。可以通过删除旧的键，然后添加新的键值对来绕过。


SetGlobalTimeDilation()
- 通过在关卡蓝图中，将该值设为0.1，可以查看脚是否踩实了。
- 通过将游戏的速度放缓，可以看到更多细节的东西。
- **如果想要放慢游戏速度来看一些游戏状态，那么通过 SetGlobalTimeDilation(0.1) 是一个好方法。**


++/--
- 自增1的蓝图节点，以前还不知道。
- 自减1

== 
- 可以在多个变量之间进行比较，比如 Vector，Rotation
- 可以提供一个容差，比如 0.01，当两个值非常接近时就认为是相等的。

Cast To
- 使用 PureCast 可以减少一个输入端。


RandomUnitVectorInConeInDegrees()
- 可以在一个锥形内随机产生矢量。非常好用。
- 注意找一下其他类似的辅助函数。【重要】【待做】
- 位于 Kismet Math Library


RInterpTo(Current, Target, DeltaTIme, InterpSpeed)
- 可以在 Current 和 Target 之间进行平滑插值。
- 使用 InterpSpeed 来设置插值速度。


GetDistanceTo(Target, OtherActor)
- 获取两个 Actors 之间的距离，非常方便。


SelectInt(A,B,Pick)
- 通过 Pick 在 A 和 B 之间选择一个值然后输出，会比 Branch 更好用一些。

Select
- 可以通过右键，修改其输入针的类型，比如 text，方便输入。

GetDotProductTo(Target, OtherActor)
- 从 Target 看向 OtherActor，Target 是否可以看见 OtherActor。
- GetDotProductTo(Player, Enemy) > 0，敌人在玩家正面
- GetDotProductTo(NPC, Player) < -0.25，NPC 看不到玩家

## 计时器 Timer

比如有这样一个时间序列 {1, 2, 3, 2}，我想要每个元素都延迟一下，再执行。

方法一：刚开始，我以为这种执行可以

```c
ForEach()
{
    dt;
    Delay(dt);
    Foo();
}
```
但是没有成功，网上字节写一个 DelayedForEach，来解决这个问题，但是每一次只能延迟固定的时长，无法满足需求。

[Unreal Engine 4 Tutorial: For Each Loop With Delay](https://www.youtube.com/watch?v=zh8xx9abzZY)


方法二：使用计时器。创建一个递归调用。基本是如下这样的结构，OK。
```c
void PrintData()
{
    if(condition)
        break;
    Foo();
    SetTimerByFunctionName("PrintData");
}

```

