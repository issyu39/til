## ViewModelの概要

ライフサイクルを意識した方法で UI 関連のデータを保存し、管理するためのクラス。  
ViewModelクラスを使用すると、画面の回転などの設定の変更後にデータを引き継ぐことができる。  

Activityの再生成によってUI関連のデータが破棄されるため、データを再取得する必要がある。  
Activityが`onSaveInstanceState()`メソッドを使用して onCreate() のバンドルからデータを復元することもできるが、このアプローチが適しているのは少量のデータの場合に限られる。  
Viewデータの所有権をUIコントローラのロジックから切り離すことで、複雑さが軽減され、効率性が高まる。

### ViewModelのライフサイクル
<img width="420" alt="スクリーンショット 2020-12-02 15 49 57" src="https://user-images.githubusercontent.com/16067422/105327413-35fef480-5c12-11eb-849a-5e049787e703.png">

**ViewModelの概要**   
https://developer.android.com/topic/libraries/architecture/viewmodel?hl=ja  
