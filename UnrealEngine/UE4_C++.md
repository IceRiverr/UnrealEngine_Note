> 初稿：2020年09月25日

## 技巧
读 C++ 代码时，直接搜索 UPROPERTY，可以大略估计出属性的数量。比如 Actor，其中有 

## 添加类
```c++

#include "Hello.generated.h"

UCLASS()
class Hello : public UObject
{
    GENERATED_BODY()
public:
    UPROPERTY()
    float score;

    UPROPERTY()
    void Foo();
}

```

注意：
- #include "Hello.generated.h" 只能是最后一个被 include 进来的文件
- Hello.generated.h 是 UHT 通过分析该文件生成的新的附加代码，用来提供反射信息。

## 参考

https://docs.unrealengine.com/en-US/Engine/Blueprints/TechnicalGuide/ExtendingBlueprints/index.html

https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Reference/index.html

https://docs.unrealengine.com/en-US/Gameplay/GameplayAbilitySystem/index.html
