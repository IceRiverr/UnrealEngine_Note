> 初稿：2020年11月20日

## 方法

将 Roughness, ambient occlution and metalness pack 成一张纹理，可以减少纹理采样数量。Masks(no sRPG)

右键材质节点，可以给 Node Comment。

如果想要使用分支
- 使用 static branch，这会产生两份shader。
- 申明一个 float 变量 w，c = Round(w)，然后使用这个结果在两个值之间插值，lerp(a,b,c)，则 0 就选择 a，1 则选择 b，实现了分支的效果。

## DynamicMaterialInstance
CreateDynamicMaterialInstance()
SetDCS
Change

## 教程

[UE4: Detailed Texture Blending with HeightLerp and Vertex Painting](https://www.youtube.com/watch?v=dghCetkArJI)
- 使用 heightLerp + VertexColor 来混合色彩。

