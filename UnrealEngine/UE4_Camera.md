> 初稿：2020年11月19日

和 UE4 摄像机有关的控制会被放在这里进行整理。

## 摄像机的一些功能
- LensEffect
- CameraAnim，CameraManager->PlayCameraAnim()
- CameraFade
- CameraShake
  
## 一些技巧
CineCamera 也有 ConstrainAspectRatio 的选项，将其不勾选，并可以k帧，这样就可以不显示黑边了。

CameraAnim 将会被替换，所以找出 LevelSequence 的替代方案。【待做】

## 参考
[UE4 ActionGame知识点总结3-Camera系统解析](https://blog.csdn.net/hui211314ddhui/article/details/80764670)

[UE4摄像机系统解析](https://jerish.blog.csdn.net/article/details/68947410?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai)

