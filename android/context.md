## Android開発におけるContext
ライフサイクルに合わせて、適切なContextを使用する

### Activity Context

- ActivityのLifecycleに従っている
- Acticityが終了したにも関わらずContextへの参照が残っていると、メモリリークが発生する可能性あり

### Application Context

- Viewまわりのメソッドで使用すると、Activityに設定したThemeが適用されない
-  AlertDialog.Builderで使用するとクラッシュする可能性あり
http://revetronique.blog.fc2.com/blog-entry-232.html?sp
- アプリケーション全体のライフサイクルで使用する場合のみ、十分に検討した上でApplicationContextを使う
- ライブラリの初期化では必ずApplicationContextを使う

## Contextの取得方法
- Activity の this
  - Activityが取得できる
- Activity や Application の getApplicationContext()
  - Applicationが取得できる
- View や Fragment の getContext()
  - Activityが取得できる

## Zennのリンク(投稿済み)
https://zenn.dev/issyu_39/articles/5225144dae22efc162fd
