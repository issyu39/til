## AutoCompleteTextView
<img width="620" alt="スクリーンショット 2020-12-02 15 49 57" src="https://user-images.githubusercontent.com/16067422/105197110-9f81f300-5b7f-11eb-9ce3-adca73aedbe5.jpg">

入力中のテキストに対して、部分的に一致する候補をドロップダウン表示して選択させる自動補完機能を提供するためのView

- `setAdapter(T adapter)` ドロップダウンに表示する候補のリストを設定
- `setCompletionHint(Char)` ドロップダウンリスト最下段に表示されるヒント
- `setThreshold(int)` 自動補完開始までの文字数(0以下は指定できない)

### ドロップダウン表示のレイアウト調整

- `android:dropDownAnchor` ドロップダウンを固定するためのView
- `android:dropDownHeight` ドロップダウンの高さ
- `android:dropDownWidth` ドロップダウンの幅

**AutoCompleteTextView | Android Developers**  
https://developer.android.com/reference/android/widget/AutoCompleteTextView
