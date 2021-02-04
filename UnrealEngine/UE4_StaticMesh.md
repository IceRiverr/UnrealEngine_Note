> 初稿：2020年09月24日

## UStaticMesh
`class UStaticMesh : public UStreamableRenderAsset`
包含有静态的几何结构，由一组多边形组成，一般是三角形。

## UStaticMeshComponent
包含
`class UStaticMesh* StaticMesh;`
网络复制的函数为
`OnRep_StaticMesh`，
并且通过
`SetStaticMesh(UStaticMesh* NewMesh)`
- 更新 渲染状态
- 更新 物理状态
- 更新 Streaming 状态
- 更新 导航数据
- 更新 包围球

