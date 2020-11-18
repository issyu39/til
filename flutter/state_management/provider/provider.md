## Provider
https://pub.dev/packages/provider


## なぜProviderを使うのか

- StatefulWidgetをStatelessWidgetに置き換えるため
  - StatefulWidgetの方が比較的パフォーマンスが悪くなる(適切にリビルドをかけないと、更新のたびにWidget全体で更新がかかる)
- Viewとロジックを疎結合にするため(関心の分離)
- DIの役割を持つ

## 値の取得方法

### context.watch()
- ビルド中のみ可
- 受け取ったデータを元にUIの構築を行う時に使う（Textにデータを渡す時など）

### context.read()
- ビルド中は不可
- 受け取ったデータを元にUIの構築を行わない時に使う（クリック時の処理など） 

これらを使い分けることによって、不必要な画面の再描画を防ぐ

## 関連
Riverpod

https://pub.dev/packages/riverpod
