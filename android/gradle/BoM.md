## BoM(Bill of Materials)
プロジェクトで利用するライブラリーのバージョンを指定して同期させるMavenの仕組み  

### 導入する利点
- 最適な依存ライブラリのバージョンを自動指定してくれる
- バージョンを個別に指定する記載が不要になり、build.gradleがスッキリする
- 関連ライブラリをまとめてアップデートすることが出来る
- ライブラリの特定バージョンの組み合わせによる不具合がなくなる

## 導入例

```Groovy
dependencies {
    implementation platform("org.jetbrains.kotlinx:kotlinx-coroutines-bom:1.4.2")
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-play-services'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-test'
}
```

## BoM一覧
- Kotlin Libraries Bill of Materials  
https://android.benigumo.com/20201204/bom/ 

- Kotlinx Coroutines BOM  
https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-bom

- firebase-bom   
https://maven.google.com/web/index.html?authuser=0&q=bom#com.google.firebase:firebase-bom

- OkHttp BOM  
https://mvnrepository.com/artifact/com.squareup.okhttp3/okhttp-bom

## 参考  
https://qiita.com/ikemura23/items/ddbe45586503971317ba
