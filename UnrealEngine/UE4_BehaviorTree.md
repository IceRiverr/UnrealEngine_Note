> 初稿：2020年10月16日

今天在看教程时，看到了虚幻 AI 的设计，我非常的陌生，我需要进行完整的学习。

## 一些技巧
按下引号键，可以 Debug AI 功能。
然后按3可以开启EQS显示，然后按小键盘的除号，可以将 EQS 选中的消息关掉。

调试 AI 时，在游戏运行时，将行为树面板打开，可以观察行为树的运行状况，并且注意黑板中值的状况。

## 一些问题

动画状态机，AI行为树，之间的状态如何同步？【问题】

AI 行为树如何驱动动画状态机的更新？

## 基础概念

Behavior Tree 行为树
- 行为树的执行顺序是从上到下，从左到右。具体执行顺序可以在节点的右上角标识出来。

Blackboard 黑板
- 存储数据供行为树使用。
- 存储的值被称为 Blackboard Keys

Composite 组合节点
- 进行流程控制的节点
- 是分支节点，可以连接不同的逻辑子树
- 基本的组合方式是：Decorators-->Composite-->Services。
- 使用多个 Decorators 来调整分支的入口，决定该分支是否可以进入，就像一些门。
- 使用多个 Services，只有组合节点的子节点执行时，才激活 Services 的执行。

Composite 的种类
- Selector 选择执行，选择器
  - 只要一个子节执行成功，则成功。如果都失败，才失败。
  - 选择子节点中的一个执行
- Sequence 序列执行
  - 从左到右依次执行其子任务。
  - 只有第一个执行成功才会执行第二个，其他类推。
- Simple Parallel
  - 只能有两个子任务，左分支是一个单独的任务，有分支可以是一个子树。
  - 执行：当A在执行时，B也在执行。
  - 当攻击敌人时，也走向敌人。
  - 

Decorator 装饰器
- 可以看作是条件判断（conditional）
- 可以被附着在组合节点或者任务节点上。
- 通过这个条件，可以决定其附着的节点是否可以执行。

常见的 Decorator 节点
- Blackboard
  - 检查黑板中的键值是否别设置过，然后通过 IsSet 和 IsNotSet 产生一个分支判断。
  - 用途：是常见的 bool 判断分支点。
  - 其中的 IsSet 和 IsNotSet 容易产生误解。其实是为了照顾 bool，Object，Actor 等不同的变量。
    - Bool 值，IsSet 代表 True，IsNotSet 为 False。
    - Object 值，IsSet 代表有值，IsNotSet 为 null。
- Does Path Exist
  - 检查黑板中的键值A-B之间是否有通路。
- Custom Decorator
  - 可以自定义一些装饰器
- Force Success
  - 使附着的节点必然成功返回。
  - 用途：？？？

Service 服务
- 被用来关联在 Composite 节点上
- 可以设定执行频率，周期性的执行更新操作
- 只要子树中有节点在执行，那么该节点附着的服务就会被激活。

一般来说 Behavior Tree（行为树） 和 Blackboard（黑板） 一起使用。
行为树提供逻辑，黑板提供数据。

Tasks 节点
- Rotate to face BB Entry
  - 转向特定目标
  - 黑板的键可以使 Vector、Object 或 Actor。

Custom Task
- 一般的逻辑在 ExecuteAI() 中执行，并且需要连接 FinishExecute 节点，成功返回。
- 如果 ExecuteAI() 中没有成功返回，那么 TickAI() 就会一直执行，其执行频率和游戏频率一致。

## 设置行为树的过程
一个虚幻的AI系统包括
- 一个被控制的 Pawn 或者 Character，BP_Enemy
- 一个自定义的 AIController，BP_EnemyController。
- 一个自定义的 Behavior Tree，BT_Enemy。

配置 AI 的过程：
1. 分别创建 BP_Enemy，BP_EnemyController，BT_Enemy
2. 在 BP_Enemy 的 AIControllerClass 中设置 BP_EnemyController。
3. 在 BP_EnemyController 的 OnPossess() 函数中，RunBehaviorTree(BT_Enemy) 即可。

## UE4 的行为树是事件驱动的
行为树被动监听黑板中的数据是否被更新，而不是每帧都去计算。

使用条件性的装饰其，有如下优点
- 这些 Decorators 放在组合节点的头部，就像一些开关，更加直观。
- 将装饰节点从 Tasks 节点中抽离出来，可以明确的知道哪些行为正在被调度。

Decorators 做为组合节点的看门者，在底层可以作为监听者，去监听黑板中数据的变化，从而优化计算。

Concurrent Behaviors，通过 Services 代替了其他行为树系统中的并行行为。

## 参考
[UE AI Doc](https://docs.unrealengine.com/en-US/Engine/ArtificialIntelligence/index.html)

UE4内的AI
- [行为树、状态机与状态设计模式](https://zhuanlan.zhihu.com/p/88065182)，
- [基于状态设计模式的AI逻辑架构](https://zhuanlan.zhihu.com/p/88321198)，
- [群集AI（战斗AI）](https://zhuanlan.zhihu.com/p/88346311)

《游戏AI程序设计实战》-王磊

[浅析UE4-BehaviorTree的特性](https://zhuanlan.zhihu.com/p/139514376)

[行为树 Behavior Tree 原理](https://blog.csdn.net/liqiangeastsun/article/details/79096684)
- 将选择节点和顺序节点讲明白了
> 选择节点(Select)，遍历方式为从左到右依次执行所有子节点，只要节点返回 Fail，就继续执行后续节点，直到一个节点返回Success或Running为止，停止执行后续节点。如果有一个节点返回Success或Running则向父节点返回Success或Running。否则向父节点返回 Fail。
> 
> 注意：当子节点返回 Running时，除了停止实行后续节点、向父节点返回 Running 外，还要保存返回Running 的这个节点，下次迭代则直接从该节点开始执行。
> 
> 如果选择节点有记录正在 Running的节点，则遍历时就要从上次记录的Running节点开始，而不是从最左边第一个节点开始执行。其他逻辑不变

> 顺序节点(Sequence),它从左向右依次执行所有节点，只要节点返回Success，就继续执行后续节点，当一个节点返回Fail或 Running 时，停止执行后续节点。向父节点返回 Fail 或 Running，只有当所有节点都返回 Success 时，才向父节点返回 Success。
> 
> 与选择节点相似，当节点返回Running 时，顺序节点除了终止后续节点的执行，还要记录返回 Running的这个节点，下次迭代会直接从该节点开始执行。

