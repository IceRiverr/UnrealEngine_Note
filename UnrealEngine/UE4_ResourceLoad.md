> 初稿：2020年09月24日

## 资源引用
### 参考
[UE4 Referencing Assets](https://docs.unrealengine.com/en-US/Programming/Assets/ReferencingAssets/index.html)  
[知乎 UE4引用资源小结](https://zhuanlan.zhihu.com/p/64485997)  
[Aery的UE4 C++游戏开发之旅（4）加载资源&创建对象](https://www.cnblogs.com/KillerAery/p/12031057.html)

常见的属性引用可以分为，**Hard Reference（强引用）、Soft Reference（弱引用）** 两类。

UE4 中资源引用可以分为如下4类。

### 1. 直接属性引用 Direct Property Reference
这种方式最常用，代码如下，AStrategyBuilding
```c++
/** construction start sound stinger */
UPROPERTY(EditDefaultsOnly, Category=Building)
USoundCue* ConstructionStartStinger;
```
在 AStrategyBuilding 被 Spawn 出来时，其所引用的资源也被同步加载出来。

### 2. 构造时引用 Construction Time Reference
当程序员在构建阶段就明确的知道需要加载哪些资源时，就可以在构造函数中写死。

代码来自 StrategyGame
```c++
/** gray health bar texture */
UPROPERTY()
class UTexture2D* BarFillTexture;

AStrategyHUD::AStrategyHUD(const FObjectInitializer& ObjectInitializer) :
    Super(ObjectInitializer)
{
    static ConstructorHelpers::FObjectFinder<UTexture2D> BarFillObj(TEXT("/Game/UI/HUD/BarFill"));
    ...
    BarFillTexture = BarFillObj.Object;
    ...
}
```
强引用的需要注意的是：
- 当对象被加载或实例化事，其引用的资源也被加载。
- 如果多个资源同时被加载，内存会爆炸，并且会卡死游戏。

### 3. 间接属性引用 Indirect Property Reference
间接引用不存放资源，知识存储资源的路径，或者模板，在使用时手动 LoadObject、StaticLoadObject、FStreamingManager 来加载资源。

使用 `TSoftObjectPtr`，`TSoftClassPtr`，来引用资源。

```c++
UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category=Building)
TSoftObjectPtr<UStaticMesh> BaseMesh;

UStaticMesh* GetLazyLoadedMesh()
{
    if (BaseMesh.IsPending())
    {
        const FSoftObjectPath& AssetRef = BaseMesh.ToStringReference();
        BaseMesh = Cast< UStaticMesh>(Streamable.SynchronousLoad(AssetRef));
    }
    return BaseMesh.Get();
}
```

### 4. 直接加载 Find/Load Object
LoadObject() 会首先执行 Find 然后才加载。

通过这个函数，可以自己拼接一个资源路径，然后在 Runtime 加载。

```c++
AFunctionalTest* TestToRun = FindObject<AFunctionalTest>(TestsOuter, *TestName);
GridTexture = LoadObject<UTexture2D>(NULL, TEXT("/Engine/EngineMaterials/DefaultWhiteGrid.DefaultWhiteGrid"), NULL, LOAD_None, NULL);

DefaultPreviewPawnClass = LoadClass<APawn>(NULL, *PreviewPawnName, NULL, LOAD_None, NULL);

DefaultPreviewPawnClass = LoadObject<UClass>(NULL, *PreviewPawnName, NULL, LOAD_None, NULL);
if (!DefaultPreviewPawnClass->IsChildOf(APawn::StaticClass()))
{
    DefaultPreviewPawnClass = nullptr;
}
```

---

## FObjectFinder()
参考：
[C++静态加载问题：ConstructorHelpers::FClassFinder()和FObjectFinder()](https://blog.csdn.net/wag2765/article/details/84769159)

静态加载，只能在构造函数中加载资源。内部调用 `LoadObject()` 。

### 资源路径
资源路径模板
`"{ObjectType}'Path/To/Asset.Asset'"`

可以通过在 Content Browser 中右键资源，Copy Reference 获取到资源的引用。

比如
`StaticMesh'/Game/Mesh/Cube.Cube'`
用来引用
`Content/Meshs/Cube.uasset`
的资源。

或者可以将前缀去掉，只有 `"/Game/Mesh/Cube"`

### 加载

```c++
auto MeshAsset =
ConstructorHelpers::FObjectFinder<UStaticMesh>(TEXT("Static
Mesh'/Engine/BasicShapes/Cube.Cube'"));
if (MeshAsset.Object != nullptr)
{
    Mesh->SetStaticMesh(MeshAsset.Object);
}
```

在 UE4 的引擎中有大量从 `Engine/Content/` 中加载资源的案例。比如 UVehicleWheel 构造函数中加载默认的碰撞网格。【推荐】

因为引擎有关的资源一般都存在，所以没有比较 Obejct 是否为 null。
```c++
#include "UObject/ConstructorHelpers.h" // 这个必须包含

static ConstructorHelpers::FObjectFinder<UStaticMesh> CollisionMeshObj(TEXT("/Engine/EngineMeshes/Cylinder"));
CollisionMesh = CollisionMeshObj.Object;
```

## FClassFinder()
静态加载 UClass，只用于构造函数中加载。内部调用 `LoadObject()` 。

加载继承自 Actor 的蓝图。
```c
static ConstructorHelpers::FClassFinder<AActor> UnitSelector(TEXT("Blueprint'/Game/Blueprints/MyBlueprint.MyBlueprint_C'"));
TSubclassOf<AActor> UnitSelectorClass = UnitSelector.Class;
```
注意：
- 资源名，蓝图类必须有 `_C` 的后缀。
- 或者将前缀去掉，即 `TEXT("/Game/Blueprints/MyBlueprint")` 。
- FClassFinder<T> 中的末班，不能使用 UBlueprint，只能使用蓝图所继承的父类，比如此处为 Actor。

```c
static ConstructorHelpers::FObjectFinder<UClass> bpClassFinder(TEXT("Class'/Game/dice/D4_Dice_BP.D4_Dice_BP_C'"));
 if (bpClassFinder.Object)
 {
     UClass* bpClass = bpClassFinder.Object;
 }
```
---

参考自[CSDN: UE4 FSoftObjectPath](https://blog.csdn.net/qq_29523119/article/details/84929384)

## FSoftObjectPath
用来给类配置具体资源的路径。其内部仅仅是字符串的路径。可以按需进行资源的加载。

这种方式在 UEngine 中大量使用，将需要加载的资源使用 
`FSoftObjectPath objName` 
申明。
然后通过在 
`BaseEngine.ini` 中进行配置，就可以方便的配置各种资源。然后在引擎中按需加载。

```c++
UPROPERTY(EditAnywhere, meta = (AllowedClasses ="Material,StaticMesh"))
FSoftObjectPath  MeshName;
```
使用下面的语法，可以筛选出“材质和网格”两种特定的资源。

### 加载
`UStaticMesh* mesh = Cast<UStaticMesh>(MeshName.TryLoad());` 

或者
```c++
UStaticMesh* mesh = LoadObject<UStaticMesh>(nullptr, MeshName.ToString()); 
mesh->AddToRoot();
```

## FSoftClassPath
用来指向蓝图类。本省继承自 FSoftObjectPath，可以使用如下的语法对资源进行过滤。
```c++
UPROPERTY(EditAnywhere, meta = (MetaClass = "MyActor"))
FSoftClassPath aaa;
```
这样的变量在 UEngine 中有大量的使用。

---


## 资源加载
[Asynchronous Asset Loading](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/ProgrammingWithCPP/Assets/AsyncLoading/index.html)

