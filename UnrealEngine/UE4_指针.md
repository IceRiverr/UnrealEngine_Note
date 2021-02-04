> 初稿：2020年09月15日

## TSubclassOf

[TSubclassOf](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/TSubclassOf/index.html) is a template class that provides UClass type safety. 

UE 在 UEngine，UGameMode，AWorldSettings 中通过暴露 `TSubclassOf<T> TClass` 可以将自己写的一些子类挂载上，做必要的定制。

```c++
TSubclassOf<UDamageType> DamageType;
```
通过如上的代码，可以在 UObject，Actor，ActorComponent 中更加安全的定义 UClass* 类型的变量。

然后可以以这个变量为基础，使用 `NewObject<T>` 即可以此为参数创建对对应的对象。



## TWeakPtr
[Weak Pointers](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/SmartPointerLibrary/WeakPointer/index.html)



---
## 参考
[Unreal Smart Pointer Library](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/SmartPointerLibrary/index.html)

