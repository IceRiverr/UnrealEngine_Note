> 初稿：2021年01月26日

## 音效
UE4 的声音系统。如果使用渲染来类比，就像一个点光源，它有自己的音效，也有自己的衰减参数。并且有产生声音者和声音监听者，这样才是一个完整的音效系统。
- 声源，环境音，对话，特效音，音乐
- 介质，环境衰减
- 耳朵，音效监听者，目前默认是摄像机。

## 播放声音的方式
- AmbientSoundActor，其封装了一个 AudioComponent，或者直接将 SouncWave 拖到场景中，就会自动播放。
- 给 Actor 添加 AudioComponent 的方式。
  - 包含有，Play，SetIsPaused，FadeIn，FadeOut，Stop 等功能。
- PlaySoundAtLocation，SpawnSoundAtLocation
- 使用 LevelSequence 中的 AudioTrack 来添加音效信息。在这里可以对音乐进行整体的编辑。
- 通过 SoundCue 来控制音乐的循环播放。

PlayerController->SetAudioListenerOverride()
- Used to override the default positioning of the audio listener

SetSoundMixClassOverride()，

## Sound Class
代表音效所属的类别。比如背景音，环境音，对话，音效，等等。

## 参考
[UE4添加音效经验小结](https://indienova.com/u/weiweifeng/blogread/4394)

[UE4 管理游戏的音量 开关](https://blog.csdn.net/maxiaosheng521/article/details/84032659)
- 同一类型的音效要继承自同一个 SoundClass 和 SoundMix
- 对某一类型的声效，可以单独设置，SetBaseSoundMix() 和 SetSoundMixClassOverride() 来分别进行设置。

[UE4 Audio Tutorial](https://www.youtube.com/watch?v=5zDC_1LN9-o&list=PLGDuj8Cat6siiMt8wcgNov0xsZvUBfDZN&index=1)
- 这个 UE4 的音效教程只有3个，但是讲的非常清晰明白。

[DPTV UE4 Unreal Audio Tutorial](https://www.youtube.com/watch?v=tnaADbKSnbo&list=PLZ7R1eB0AmM9Foq_ZPycMleLolH1Ji7vP)
- 一共八个教程

[Unreal Engine 4 Audio Tutorial](https://www.raywenderlich.com/354-unreal-engine-4-audio-tutorial)

[Unreal Engine 4 Tutorial: Painting With Render Targets](https://www.raywenderlich.com/5246-unreal-engine-4-tutorial-painting-with-render-targets)


