## Fragmentでの戻るボタン押下時のハンドリング

- 以前はFragment用にinterfaceを定義し、Activityの`onBackPressed()`をオーバーライドして戻るボタン押下時の処理を実装していた。

### onBackPressedDispatcher 
Fragmentで以下を定義する

```Kotlin
activity?.onBackPressedDispatcher?.addCallback(this, object : OnBackPressedCallback(true) {
    override fun handleOnBackPressed() {
        // 戻るボタン押下時の処理
    }
})
```
