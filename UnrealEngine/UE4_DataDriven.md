# UE4 数据驱动 Data Driven 

> 初稿：2020年09月11日

## 功能

UE4 的数据驱动是使用 DataTable 来驱动的。就像是一个数据库。

优点：
- 缓建工作量，降低复杂度。
- 参数化数据创建，和流程
- 如果游戏的平衡性需要按照玩家反馈进行调整，比如 LOL 中英雄和装备的属性。

游戏包含哪些数据，哪些数据应该设计成表，需要行业经验。这些游戏数据，跨引擎的。

我可以分析网络游戏，比如魔兽私服，比如传奇私服。学习网络游戏教程，看网络游戏看法的书来增加这方面的经验。【待做】

可以使用 excel 来配置和导入。 Excel 表格先转化为 csv 格式再导入。

## 注意事项

数据表中引用资源的方式，TAssetPtr<UTexture> Icon。

**数据编码** UE4中在输入中文时，将编码格式改为 UCS-2 小段端编码。先ANSI 再转为UCS-2 Little Endian

UE4中的数据表，不要缓存，因为可能会重新导入数据
当数据表加载时，其中的所有资源都会被加载；lazy loaded asset。

## Data Table 使用步骤：
1. 设计表格，一个表应该包含哪些项目，比如一个 LevelUpData 包含这些数据 `{ XP, AddHP, Icon}`。
2. 或者在蓝图中创建一个 BlueprintStruct 来作为表头。
3. 或者在 C++ 中继承自 FTableRowBase 来创建表头。
4. 在 Excel 中建表，然后填数据，然后导入 UE4。或者直接在 UE4 中进行编辑。
5. 然后在 Actor 中进行资源引用和计算。比如 GetDataTableRow(DataTable, RowName)

## Data Table 引用数据表作为参数
1. 在 MyActor 定义 DataTableRowHandle Info，封装了 DataTable 和 RowName 这两个成员。
2. 在关卡中对该 Actor 进行配置，设置对应的数据表和行名。
3. 在 Actor 中使用时，第一步 BreakDataTableRowHandle 能获取 DataTable 和 RowName，第二部 GetDataTableRow() 能获取到这一行的数据，然后 Break 该结构体，就可以使用。

## 蓝图的使用方法
https://www.cnblogs.com/ghl_carmack/p/5677248.html
- GetDataTableRow() 可以获取数据表中某一行的数据
- 然后使用 BreakDataTable-->来获取其对应的数据值
- 对于TAssetPtr<> 是使用LoadAsset()异步加载
- 对于TAssetSubClass<> 使用LoadClassAsset<>()来加载

UE4中的内存管理
http://gad.qq.com/program/translateview/7182050

http://blog.csdn.net/pizi0475/article/details/48178861
UE4中资源异步加载

## DataTable导入数组的方法
,PoseID,NextPoses
295,295,"(296,303)"
296,296,"(297,299,300,301,302,303,305,306,374,309,313,572,565)"

## DataAsset
[UE4 C++结合DataTable批量快速创建DataAsset](https://zhuanlan.zhihu.com/p/152157327)
1. 继承自 FTableRowBase 的 FItemsProperties
2. 再创建 UItemBase : public UPrimaryDataAsset，其中包含 FItemsProperties。
3. 写一个函数库 UMyDataFunctionLibrary : public UBlueprintFunctionLibrary
4. 编辑器工具蓝图。EditorUtilityObject，重写其中 Run() 函数。然后右键，直接运行。

如下是一个标准的创建资源的流程

```c++
void UMyDataFunctionLibrary::CreateNewDataAsset(const FString PackagePath, const FString AssetName,const FItemsProperties& ItemProperty)
{
	//创建package
	UPackage* MyPackage = CreatePackage(nullptr, *PackagePath);

	//创建Object
	UItemBase* ItemDataAsset = NewObject<UItemBase>(MyPackage, UItemBase::StaticClass(), *AssetName, EObjectFlags::RF_Public | RF_Standalone);
	
	if (ItemDataAsset == nullptr)
	{
		return;
	}

//初始化对象==
	ItemDataAsset->ItemProperty = ItemProperty;//简要起见，直接赋值，必要时需要对拷贝赋值运算符重载
//结束初始化对象==

	FAssetRegistryModule::AssetCreated(ItemDataAsset);//告知资源浏览器

	ItemDataAsset->GetOutermost()->MarkPackageDirty();//标星号
}
```


---
## 参考：

[UE-Doc: Data Driven Gameplay Elements](https://docs.unrealengine.com/en-US/Gameplay/DataDriven/index.html)  

[UE4中使用数据表（Data Table](https://www.cnblogs.com/ghl_carmack/p/5677248.html)  

[ue4数据表的使用](http://blog.csdn.net/yangxuan0261/article/details/52078278)

[第21期 | UE4数据驱动开发 | Epic 大钊](https://www.bilibili.com/video/BV1dk4y1r752)

