## Provider
https://pub.dev/packages/provider


## なぜProviderを使うのか

- StatefulWidgetをStatelessWidgetに置き換えるため
  - StatefulWidgetの方が比較的パフォーマンスが悪くなる(適切にリビルドをかけないと、更新のたびにWidget全体で更新がかかる)
- Viewとロジックを疎結合にするため(関心の分離)
