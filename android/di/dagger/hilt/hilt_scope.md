## HiltのComponentsとScopes

### ComponentsとScopes

<img width="1041" alt="スクリーンショット 2023-06-17 13 36 04" src="https://github.com/issyu39/til/assets/16067422/05680647-6624-40c9-a63e-abcb673e248c">

Componentを指定してもScopeが指定されていなければ、インスタンスが要求されるたびに毎回再生成される  

```Kotlin
@Module
@InstallIn(FragmentComponent::class)
object FooModule {
  // This binding is "unscoped".
  @Provides
  fun provideUnscopedBinding() = UnscopedBinding()

  // This binding is "scoped".
  @Provides
  @FragmentScoped
  fun provideScopedBinding() = ScopedBinding()
}
```

### Scopeを設定する副作用
- Scopeの設定には、生成されるコードサイズとその実行時のパフォーマンスの両方にコストがかかるため、Scopeの設定は慎重に使用する
- Scopeを設定する必要があるかどうかを判断するための一般的なルールは、コードの正確性のために必要な場合にのみ設定すること
- 純粋にパフォーマンス上の理由でScopeを設定する必要があると考えられる場合は、まずパフォーマンスが問題であることを確認し、問題がある場合はScopeの代わりに`@Reusable`の使用を検討する

### `@ActivityRetainedScope`と`@ActivityScope`の違い
 `@ActivityRetainedScope`はconfiguration changesで再生成されない、`@ActivityScope`はconfiguration changesで再生成される

### 参考
- https://dagger.dev/hilt/components
- https://medium.com/digigeek/hilt-components-and-scopes-in-android-b96546cb07df  
