> 初稿：2020年09月15日

主要是对 UEngine 的理解。

## 全局配置
```c++
UPROPERTY(globalconfig)
FSoftObjectPath TinyFontName;
```
通过在属性的元数据中提供 **globalconfig**，引擎将这个值通过 BaseEngine.ini 来进行配置。

在 BaseEngine.ini 中可以找到对应的字段：
TinyFontName = /Engine/EngineFonts/RobotoTiny.RobotoTiny

而这个值，可以通过 Edit|Project Settings|General Settings|Tiny Font，来进行设置。

然后在 `void UEngine::InitializeObjectReferences()` 中进行默认字体，纹理，材质，UClass 的记在。

**通过设置 Project Settings，可以在 Engine 级别对引擎进行定制**。【结论】

