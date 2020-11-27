## FragmentでViewの参照を持つときのメモリリークを防ぐ方法
Fragment自体のライフサイクルのほうが、FragmentのViewのライフサイクルより長いので、FragmentでBindingの参照を保持するとリークしてしまう  

### 1. onDestoryViewで解放する

```
override fun onDestroyView() {
  _binding = null
}
```

### 2. AACサンプルで使っているAutoClearedValueを使う
https://github.com/android/architecture-components-samples/blob/8f536f2/GithubBrowserSample/app/src/main/java/com/android/example/github/util/AutoClearedValue.kt

### 3. DataBinding-Ktxを使う
https://github.com/wada811/DataBinding-ktx  
https://note.com/wada811/n/n9a819494b659

### 4. View.setTag, getTagを使った実装を使う
https://gist.github.com/satoshun/0185c4231983016f6afa4d8f8c423cd9

## 参考リンク
https://satoshun.github.io/2020/01/fragment-view-memory-leak/
