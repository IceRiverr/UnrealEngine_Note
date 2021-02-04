> 初稿：2021年02月03日

## 在 UMG 中获取输入的方法
1. 在 Character 或 Actor 中获取到输入，并保存下来，然后在 UMG 的 Tick 中访问 Actor，获取到输入按键。
2. OnKeyDown()，起作用的条件是，将 InputMode 设置为 UIOnly 或者 GameAndUI，这样才能获取到 OnKeyDown() 的事件。
3. 在 UMG 的 Tick() 中，通过 PlayerController->IsInputKeyDown(Key) 来实时检测输入的按键。这样可以在 Game 模式下，UI 对象也可以监听到输入。

