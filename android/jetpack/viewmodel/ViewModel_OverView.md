## ViewModelの概要

ViewModelクラスは、ビジネスロジックまたは画面レベルの状態を保持するもの。UIに状態を公開し、関連するビジネスロジックをカプセル化する役割を持つ。  
主な利点は、状態をキャッシュし、Configuration Changes後もそれを保持することであり、それによってアクティビティ間を移動するときや、画面を回転させるときなどのConfiguration Changes時に、UIがデータを再度取得する必要がなくなる。  

### ViewModelの利点
ViewModelの代わりになるのは、UIに表示するデータを保持する標準クラスである。この方法だと、アクティビティまたはNavigationのDestination間を遷移する場合に、`SavedInstanceState`を使わない限りデータが破棄されてしまう問題がある。  
ViewModelはデータ永続化のための便利なAPIを提供しているため、この問題を解決することができる。  
  
ViewModelクラスの主な利点は、以下の2つがある。  
- UIの状態を保持する   
- ビジネスロジックへアクセスできるようにする  

#### 永続性  
ViewModelが保持する状態と、ViewModelがトリガーする操作の両方を通じて、データの生存を可能にしている。      
これにより、画面回転などのConfiguration Changesによって、データを再度取得する必要がなくなる。  

#### スコープ  
ViewModelをインスタンス化する際、ViewModelStoreOwnerインターフェースを実装したオブジェクトを渡す。これは、NavigationのDestination、Navigation Graph、Avtivity、Fragment、またはインターフェイスを実装する他のタイプである可能性がある。  
ViewModelは、ViewModelStoreOwnerのLifecycleにスコープされ、ViewModelはViewModelStoreOwnerが永久に消えるまでメモリ内に残る。  
ViewModelがスコープされているフラグメントまたはアクティビティが破棄されても、それにスコープされているViewModelで非同期作業が継続される。これが永続化のポイントである。   

#### SavedStateHandle  
SaveStateHandleを使用すると、Configuration Changesだけでなく、プロセスデス後もデータを永続化することができる。   
つまり、ユーザーがアプリを閉じて、後で開いたときでも、UIの状態をそのまま維持することができるようになる。


### ViewModelの実装
- ViewModelのライフサイクルはUIのライフサイクルより長いため、View、Lifecycle、またはActivity Contextへの参照が保持されている可能性があるクラスを参照しないようにする  
- ViewModelでは、ライフサイクル対応の監視可能オブジェクト（LiveDataオブジェクトなど）への変更を監視しないようにする  

### ViewModelのライフサイクル
<img width="420" alt="スクリーンショット 2020-12-02 15 49 57" src="https://user-images.githubusercontent.com/16067422/105327413-35fef480-5c12-11eb-849a-5e049787e703.png">

**ViewModelの概要**   
https://developer.android.com/topic/libraries/architecture/viewmodel?hl=en
