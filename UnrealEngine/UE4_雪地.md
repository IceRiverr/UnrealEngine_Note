> 初稿：2020年10月15日

今天在B站看到一个在 UE4 中实现雪地的视频。所以我也想要实现一个。

[如何制作可交互雪地](https://www.bilibili.com/video/BV1yE411k7MY)

我在知乎也看到一个实现，应该是参考自这个视频，见
[虚幻4中可互动的雪地材质完整实现（1）](https://zhuanlan.zhihu.com/p/80913086)，[2](https://zhuanlan.zhihu.com/p/82682271)，[3](https://zhuanlan.zhihu.com/p/83123824)。

以下记录我的一些实现笔记。

---

## SceneTexture::CustomDepth

[虚幻4中用自定义深度完整实现描边材质（一）](https://zhuanlan.zhihu.com/p/81310476)

SceneTexture::CustonDepth，当通过 uv 值访问该纹理中的值时，如果该位置没有自定义深度，那么访问到的值为无穷大。

那么我们就可以通过 SceneTexture::SceneDepth 和 SceneTexture::CustomDepth 差来判断是否有物体被挡住，如果被挡住应该显示什么颜色。

使用 d = CustomDepth - SceneDepth 有三种情况：
- 开启 CustomDepth 的物体被挡住，则 d 为有限制。
- 开启 CustomDepth 的物体没有被挡住，则 d = 0。
- 如果没有开启 CustomDepth，那么 d 为无穷大。

同时 d = CustomDepth - SceneDepth
- 如果 d == 0，当前采样位置处有 CustomDepth，并且没有被挡住。
- 如果 d > 0 && d < 10^7，则说明采样位置处有 CustomDepth，并且被挡住了。
- 如果 d > 10^7，则说明采样到的位置没有 CustomDepth，只有原始物体。

## UE4 RenderTargets
[Content-Driven Multipass Rendering in UE4 | GDC 2017 | Unreal Engine](https://www.youtube.com/watch?v=QGIKrD7uHu8)

[Blueprint Drawing to Render Targets Overview | Live Training | Unreal Engine](https://www.youtube.com/watch?v=1Z-V1Buk6z8)

[Unreal Engine 4 Tutorial: Painting With Render Targets](https://www.raywenderlich.com/5246-unreal-engine-4-tutorial-painting-with-render-targets)

## 参考
[AAA游戏中雪的实现](https://zhuanlan.zhihu.com/p/56927127)

GPU PRO 6

[Creating Snow Trails in Unreal Engine 4](https://www.raywenderlich.com/5760-creating-snow-trails-in-unreal-engine-4)

[游戏中沙漠、雪地的轨迹、脚印实现](https://zhuanlan.zhihu.com/p/103059644)

