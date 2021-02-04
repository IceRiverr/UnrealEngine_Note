# UE4 属性系统 Property System

> 初稿：2020年09月11日


## 原型和实例
一个类对应的蓝图类即为原型。
一个类添加至关卡等的对象即为实例。

## Edit
EditDefaultsOnly 通过属性窗口来编辑，但仅能对原型编辑。

---
## 参考：
[UE4虚幻架构之属性修饰符](https://www.cnblogs.com/liuanyin/p/10282876.html)

[UE4C++定义属性修饰符总结](https://www.cnblogs.com/yukiArea/p/10282595.html)

摘录如下：
1. BlueprintAssignable  暴露该属性来在蓝图中进行赋值，用于绑定多播委托
2. BlueprintCallable  用于从蓝图中调用C++原生函数
3. BlueprintReadOnly 蓝图读取，但不能修改且与BlueprintReadWrite不兼容。
4. BlueprintReadWrite 蓝图读取和写入
5. EditAnywhere  可以在编辑器内的属性窗口编辑，在原型和实例中都能编辑
6. EditDefaultsOnly  通过属性窗口来编辑，但仅能对原型编辑。
7. EditFixedSize   仅限于动态数组，不能通过编辑属性的窗口来变更数组的长度。
8. EditInline   通过此修饰符可编辑属性查看器中的变量所引用的对象属性。（仅对对象引用可用，包括对象引用数组）。
9. EditInstanceOnly   可通过属性窗口来编辑实例，但仅能对实例而非原型进行编辑。
10. VisibleAnywhere  该属性只在属性窗口中可见，但无法被编辑。
11. VisibleDefaultsOnly  仅在原型的属性窗口中可见，且无法被编辑。
12. VisibleInstanceOnly  该属性仅在实例的属性窗口中可见，且无法被编辑。
13. AdvancedDisplay   属性被显示在细节面板的高级下拉框中

[UE4虚幻架构之属性修饰符](https://www.jianshu.com/p/a86d567be8c3)

[虚幻4属性系统（反射）翻译](https://www.cnblogs.com/ghl_carmack/p/5698438.html)

[Unreal Property System (Reflection)](https://www.unrealengine.com/zh-CN/blog/unreal-property-system-reflection)
