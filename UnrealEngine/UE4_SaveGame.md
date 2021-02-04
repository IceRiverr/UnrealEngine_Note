> 初稿：2020年12月25日

---
## 常见接口
- DoesSaveGameExist(SlotName, UserIndex) SaveGame 是否存在
- CreateSaveGameObject(SaveGameClass) 创建一个 SaveGame
- DeleteGameInSlot(SlotName, UserIndex) 删除
- LoadGameFromSlot(SlotName, UserIndex), AsyncLoadGameFromSlot()
- SaveGemeToSlot(SaveGameObject, SlotName, UserIndex), AsyncSaveGameToSlot()

## 工具
Rama，UE4 中存储系统的插件



## 博客
[Saving and Loading Your Game](https://docs.unrealengine.com/en-US/InteractiveExperiences/SaveGame/index.html)
通过继承 SaveGame 来自定义想要保存的属性。这些属性就可能是玩家状态，任务状态，场景中 NPC 的状态等等。
可以将一个游戏中需要保存的内容拆分到不同的 SaveGame。
1. Custom MySaveGame
2. Add custom properties to MySaveGame
3. AsyncSaveGameToSlot() / SaveGameToSlot() save to a slot.

AsyncSaveGameToSlot() 是一个异步操作，不会阻塞主线程。对于小一点的数据量可以退通过 SaveGameToSlot() 来解决。

SaveGame 在 Windows 中被存储在 ProjectName/Saved/SaveGames/{slotName}.sav 的文件中。

对于 Load，
先通过 DoesSaveGameExit(SlotName) 来检查 SaveGame 是否存在。
有 LoadGameFromSlot() 和 AsyncLoadGameFromSlot() 来从特定的槽加载 SaveGame 的数据，如果数据量比较大，可以添加一个 LoadingScreen，当异步加载 Completed 时，将 LoadingScreen 关闭即可。

---

[How do we save a large and complex amount of data](https://www.reddit.com/r/unrealengine/comments/70zkxe/how_do_we_save_a_large_and_complex_amount_of_data/)
> I used [this tutorial](https://michaeljcole.github.io/wiki.unrealengine.com/Save_System,_Read_&_Write_Any_Data_to_Compressed_Binary_Files/) which I believe is the same system driving his marketplace plugin. I put my save/load functions in GameInstance and have an Interface called Savable with 2 functions: Save() and Load(). When I save, I just get all actors of type Savable and call Save() on them, plus a couple extra things specific to my game.

核心的做法是：
- 在 GameInstance 中处理保存的功能。
- 创建一个 I_Savable 的接口，其中包含，Save() 和 Load() 的函数。
- 对于所有的需要进行保存的 Actor，都实现 I_Saveable 的接口。
- 在 GameInstance 中对数据进行收集并存储。

---

如果需要保存游戏中动态生成的蓝图 Actor？
首先要确定的是，哪些信息需要被保存，实际就是要将这些对象序列化。重要的是要保证序列化的顺序。
对于 Actor，保存其 ClassType，Transform，以及其他属性。有两种方式。
1. 在 SaveGame 中，保存 ActorClassArray，ActorTransformArray，以及各种属性的数组。每个数组都只保存单一的数据。
2. 构造一个 S_SaveStruct {Class; Transform}，在 SaveGame 中保存这个数据的数组。这也是一种方式。

在 [UE4 - Save and load BP Actors!](https://www.youtube.com/watch?v=UR-g8Zcrq7g) 使用的是第一种这种方式。
在 Complete-RPG 中使用的是第二种方式。

当然另一种方式是使用数据库来存储，这是网络游戏中比较通用的一种方式。

---

[Saving and loading actor data in Unreal Engine 4](http://runedegroot.com/saving-and-loading-actor-data-in-unreal-engine-4/)

[Serializing Monsters for Saving and Loading](https://interstellarechidna.com/post/serializing-actors/)

[What is the best way to handle Saving/Loading an Array of Objects?](https://answers.unrealengine.com/questions/35618/savingloading-an-array-of-objects.html)



