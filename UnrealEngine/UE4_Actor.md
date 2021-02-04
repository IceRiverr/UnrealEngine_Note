> 初稿：2020年09月15日

## Actor 的性质：
Actor以及子类，可以放置在关卡中，或者动态创建。

Actor 对象，sizeof() 有多大？【问题】

Actor 的用途
- 基础的逻辑类，比如 CameraDirector，用来切换不同的摄像机。
- 使用 Actor 来表现受击表现，比如声效，粒子的 GameCue，当物体被攻击时，就在关卡中创建一个这样的 GameCue，非常方便创建多种不同的 GameCue 的效果。
- 
[【UE4】对gamePlay架构中actor的理解](https://blog.csdn.net/WadaFak/article/details/107558474)

## 隐藏 Actor

```c++
void AHiddenActor::DisableActor()
{
    SetActorHiddenInGame(true);
    SetActorEnableCollision(false);
    SetActorTickEnabled(false);
}

```

## 修改 Tick

当然可以调用 
- SetComponentTickEnabled() 关闭特定的组件
- SetComponentTickInterval(TickInterval) 设置组件更新间隔

Actor
- SetActorTickEnabled()
- SetActorTickInterval()

可以在 Actor Tick 属性组中进行对应的设置。

TickGroup 和 Tick 依赖。【待做】

## Actor 的初始化流程
[Actor Lifecycle](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/Actors/ActorLifecycle/index.html)

[UE4的Actor生命周期(1) 之 SpawnActor , SpawnActorDeferred 产生 Actor 和 AActor 初始化的执行流程](https://blog.csdn.net/qq_29523119/article/details/86368182)

PostLoad
- 用于放置在 Level 中的角色。
- 可用于执行自定义的版本控制和修正行为

PostActorCreated
- 用于 SpawnActor 创建以后，并在 Construction Script 调用以前。
- 如果 Actor 有根组件，则其 Location 和 Rotation 已经被设置
- 此时 Native Components 已经被设置
- 一些 Constructor 的行为可以放置在这里
- 在 AAtomosphericFog 的这个函数中，记性 AtomosphericFogComponent->InitResource() 的操作
- 在 VREditor_ 的这个函数中，进行 Native Components 的进一步配置。

OnConstruction
- 在 ExecuteConstruction 的末尾调用
- 此处会调用蓝图构造脚本
- 这里是蓝图组件和蓝图变量初始化的地方
- 此时蓝图创建的组件已经创建和注册成功

BeginPlay()
- BeginPlay() 是 Tick() 之前的最后一步操作。此时 Actor 的构造，以及组件的创建都已经完成，可以安全的进行相互引用。

需要进行测试。【待做】
- 写一个 c++ Actor，然后申明一些 c++ object，看一下其执行流程。以及 NativeComponent 的执行流程。

Component 的 Create-Register-Init【问题】

## Deferred Spawn

An Actor can be Deferred Spawned by having any properties set to "Expose on Spawn."
1. SpawnActorDeferred - meant to spawn procedural Actors, allows additional setup before Blueprint construction script.
2. Everything in SpawnActor occurs, but after PostActorCreated the following occurs:
   - Do setup / call various "initialization functions" with a valid but incomplete Actor instance.
   - FinishSpawningActor - called to Finalize the Actor, picks up at ExecuteConstruction in the Spawn Actor line.

```c++
AShooterProjectile* Projectile = Cast<AShooterProjectile>(UGameplayStatics::BeginDeferredActorSpawnFromClass(this, RowBombInfoDataTable->ShooterProjectileClass, SpawnTM));
			if (Projectile)
			{
				Projectile->Instigator = Instigator;
				Projectile->SetOwner(this);
				......
				Projectile->SetConfig(Weapon);

			}
UGameplayStatics::FinishSpawningActor(Projectile, SpawnTM);
```

触发条件是，如果 Actor 包含有 ExposeOnSpawn 的参数。
1. 允许在构造脚本前有自定义的设置步骤。
2. 调用不同的 Init() 初始化函数，但是 Acotr 对象时有效的但不完全。
3. 通过 FinishSpawningActor 来完成初始化，其中会调用 ExecuteConstruction。

ExposeOnSpawn
- 蓝图中勾选 ExposeOnSpawn，这样在 SpawnActor 时可以直接设置这个值
- C++ 中，`UPROPERTY(EditAnywhere, Meta = (ExposeOnSpawn = true)) bool bTestVal = fasle;`，来暴露
- 通过 SpawnActorDeferred<> 来构建，然后初始化 bTestVal，然后调用 FinishSpawningActor() 来完成初始化。

[UE4 Expose On Spawn 的使用](https://blog.csdn.net/l346242498/article/details/76682615)

## 自定义 ALevelScriptActor
[UE4的(UE4 4.20 ) UE4的GamePlay世界框架](https://blog.csdn.net/qq_29523119/article/details/89463926)

在 UE4 中写 ALevelScriptActor 的子类，然后在 UE4 打开关卡蓝图，在 Class Settings 中设置父类即可。

## 对象遍历
for (TActorIterator<AActor> it(pWorld); it; ++it)

for (TObjectIterator<AActor> it; it; ++it)

for (FConstPawnIterator it = pWorld->GetPawnIterator(); it; ++it)

