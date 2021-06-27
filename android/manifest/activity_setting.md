## マニフェストファイルの`<activity>`
  
### `android:configChanges`
Activityで処理する設定変更のリストを指定する。  
実行時に設定の変更が発生すると、デフォルトではActivityはシャットダウンおよび再起動されるが、この属性で設定を宣言しておくと、Activityの再起動を防止できる。 
代わりに、Activityは実行中のままになり、Activityの`onConfigurationChanged()`メソッドが呼び出される。  
  
#### 主なパラメータ
- `orientation` 画面の向きが変更されたとき、またはユーザーが端末の向きを変えたとき。
- `screenSize` 現在使用可能な画面サイズが変更されたとき。これは、現在のアスペクト比に応じた、現在使用可能なサイズの変更を表す。したがって、ユーザーが横向きと縦向きを切り替えると変更される。  

#### 実用例
- WebViewで画面回転時にリロードを防ぐ
```xml
<activity
    android:name=".MainActivity"
    android:configChanges="orientation|screenSize">
</activity>
```  
