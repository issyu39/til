## StatefulWidget
State(状態)を持つWidgetであり、 ユーザー操作や通信によって動的に変化する  
https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html

### StatefulWidgetの構成 
- **Widgetクラス**    
StatefulWidgetクラスを継承して定義する。  
Stateクラスのインスタンスを作成して返す、`createState`というメソッドを実装する必要がある。  

```Dart
class MyHomePage extends StatefulWidget {
  const MyHomePage({
    Key? key,
    this.title
  }) : super(key: key);
  
  final String title;
  
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

- **Stateクラス**  
Stateクラスを継承して作成する。Widgetクラスは`<>`で指定する。    
Stateクラスには`build`というメソッドが用意されており、Stateを生成する際に呼び出され、ここでStateとして表示されるWidgetを生成して返す。   
Stateが更新される度に、`build`で新たな表示内容が生成され画面に表示される。

**build method**  
https://api.flutter.dev/flutter/widgets/State/build.html

#### initStateメソッド  
Stateクラスで利用するプロパティの初期化処理を`initState`で行う。  
`initState`はインスタンスが作成され、Widgetのツリー構造への組み込みが完了した時点で呼び出される。    
このため、コンストラクタではウィジェットツリーが完了していないためにエラーになるような処理(他のWidgetの参照など)も、`initState`では問題なく実行できる。

#### State更新とsetState
`setState`はStateの更新をStateクラスに知らせる働きをする。  
以下のコード例では、`_message`を変更することで、更新時に`build`が再実行されTextの値が変更される。


```Dart
class _MyHomePageState extends State<MyHomePage> {
  String _message;
  
  @override
  void initState() {
    super.initState();
    _message = 'Hello!';
  }
  
  @override
  void _setMessage() {
    setState(() {
      _message = 'Tapped!';
    });
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        // MyAppで定義されたtitleは、widget.titleで取得できる
        title: Text(widget.title),
      ),
      body: Text(
        widget.message,
        style: TextStyle(fontSize: 32.0),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _setMessage,
        toolTip: 'set message.',
        child: Icon(Icons.star)
      ),
    );
  }
}
```

### StatelessWidgetからStatefulWidgetを呼び出す
```Dart
class MyApp extends StatelessWidget {
  final title = 'Flutter Sample';

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: const MyHomePage(
        title: this.title,
      ) 
    );
  }
}
```

## 参考リンク  
**FlutterのWidgetツリーの裏側で起こっていること**  
https://medium.com/flutter-jp/dive-into-flutter-4add38741d07
