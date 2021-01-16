## Android用アサーションライブラリ

### Truth  

- Google 製
- AndroidX Test Library が Truth 向けの拡張を用意している
- 基本的には Truth.assertThat に検証したいオブジェクトを渡し、検証メソッドを呼び出す

```Java
assertThat(1 + 1).isEqualTo(500);
```

- 検証メソッド
```
・ 等しい (isEqualTo(Object))
・ 等しくない (isNotEqualTo(Object))
・ 指定値より大きい (isGreaterThan(T))
・ 指定値より小さい (isLessThan(T))
・ 値がTrue (isTrue())
・ 値がFalse (isFalse())
・ 値がnull (isNull())
・ 値がnullではない (isNotNull())
```

https://truth.dev/  
https://proudust.github.io/20190703-how-to-use-truth/

### Hamcrest 
- JUnit4 の標準
- Kotlinは「is」が予約語になっているので、Hamcrestで記述すると「｀is｀」になってしまう問題  

### AssertJ 
- メソッドチェーンで書ける
- チェック対象の値の型を見て、使用可能なメソッド一覧が候補として、表示される(IDE補完機能)

### Kluent
- Kotlinに特化したJunitアサーションライブラリ    

Docs: https://markusamshove.github.io/Kluent/    
Github: https://github.com/MarkusAmshove/Kluent    
