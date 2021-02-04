> 初稿：2021年01月07日

## UE4 中的物理材质是怎么实现的？
UE4 中通过材质来创建需要的地形层名。
然后再地形层编辑器中，创建每个层自己的 LandscapeLayerInfoObject，在其中可以设置每个层的 PhysMaterial，这样就可以被引擎的物理系统识别到。
LineTraceByChannel() 可以通过返回的 PhysMat 来查询 SurfaceType，然后按照不同的表面类型来播放声音，产生特定的粒子效果。

在 UE4 中，通过 Project Settings/Physics/Physics Surface，可以设置物理表面的类型，一个项目一共可以有 62 种自定义物理表面类型。然后就可以通过查询物理系统，获得当前表面的信息，从而基于不同的表面产生不同的玩法。

## 雪地的脚印，弹坑，怎么实现？

## 参考教程
[解构-UE4-地形自动材质（作用，原理，使用方式）01: 技术解析](https://www.bilibili.com/video/BV1GQ4y1M7ba?t=2)
[解构-UE4-地形自动材质（作用，原理，使用方式）02 层基础功能](https://www.bilibili.com/video/BV1r541167gC?t=1280)

