## UIレイヤ

UIの役割は、アプリデータを画面に表示することであり、また、ユーザーインタラクションの主要なポイントとして機能することである。  
ユーザーインタラクション（例: ボタンを押す）や外部入力（例: ネットワーク応答）によってデータが変更されるたびに、UIを更新してそのような変更を反映させる必要がある。UIは、実質的には、データレイヤから取得されたアプリの状態を視覚的に表したものである。  

一般的に、データレイヤから取得するアプリデータの形式は、表示する必要がある情報の形式とは異なる。たとえば、UIでは、データの一部だけが必要になる場合や、ユーザーに関連する情報を提示するために2つの異なるデータソースの統合が求められる場合がある。
UIを完全にレンダリングするために必要なすべての情報をUIに渡す必要がある。UIレイヤは、アプリデータの変更をUIが提示できる形式に変換して表示するパイプラインである。

<img width="600" alt="スクリーンショット 2020-11-24 16 38 44" src="https://user-images.githubusercontent.com/16067422/146179702-88574702-3167-4031-8f58-63100e4f0ca5.png">

### Uiレイヤのアーキテクチャ
UIレイヤの役割として、次のステップを実行する必要がある。
1. アプリデータを使用し、UIが簡単にレンダリングできるデータに変換する
2. UIがレンダリングできるデータを使用し、ユーザーに表示するUI要素に変換する
3. このような UI 要素の集合体からのユーザー入力イベントを使用し、必要に応じてその結果をUIデータに反映させる
4. ステップ1～3を必要な回数だけ繰り返す

### UI状態を定義する
ユーザーが目にするものが`UI`であるとすれば、ユーザーが目にするべきであるとアプリがみなすものが`UI状態`である。同じコインの両面のように、`UI`は「UI状態を視覚的に表したもの」であり、`UI状態`が変更されると、すぐに`UI`に反映される。

<img width="540" alt="スクリーンショット 2024-07-20 15 49 23" src="https://github.com/user-attachments/assets/24db5c8b-9396-42d8-8703-b0f6ecc4a6c6">

例えばニュースアプリの要件を満たすため、UIを完全にレンダリングするのに必要な情報は、次のように定義される`NewsUiState`データクラスにカプセル化できる。
```Kotlin
data class NewsUiState(
    val isSignedIn: Boolean = false,
    val isPremium: Boolean = false,
    val newsItems: List<NewsItemUiState> = listOf(),
    val userMessages: List<Message> = listOf()
)

data class NewsItemUiState(
    val title: String,
    val body: String,
    val bookmarked: Boolean = false,
    ...
)
```

#### 不変性
上記の例では、UI状態の定義は不変である。その重要なメリットは、不変オブジェクトがその時点でのアプリの状態を確実に提供できることです。これにより、UIは唯一の役割である、状態を読み取り、それに応じて自身のUI要素を更新する役割に集中できる。したがって、UI自体がそのデータの唯一のソースでない限り、UI内でUI状態を直接変更すべきではない。この原則を破ると、同じ情報について信頼できるソースが複数発生し、データの不整合やわかりにくいバグにつながる。  
たとえば、ケーススタディにおいて、UI状態の`NewsItemUiState`オブジェクトの`bookmarked`フラグがActivityクラス内で更新された場合、そのフラグは、記事の「ブックマーク済み」ステータスのソースとしてデータレイヤと競合することになる。不変のデータクラスは、この種のアンチパターンを防止するためにとても役立つ。  
データのソースまたはオーナーのみが、それらが公開するデータを更新する責任を負うべきである。

### 単方向データフローで状態を管理する
#### 状態ホルダー
UI状態を生成する責任を負い、そのタスクに必要なロジックを格納するクラスを、状態ホルダーと呼ぶ。状態ホルダーのサイズは、ボトムアプリバーのような単一のウィジェットから、画面全体またはナビゲーションデスティネーションまで、対応する管理対象のUI要素のスコープに応じて様々である。   
後者の場合、一般的な実装はViewModelのインスタンスであるが、アプリの要件によっては単純なクラスで十分な場合もある。
ViewModeは、データレイヤにアクセスして画面レベルのUI状態を管理する場合に推奨される実装であり、構成変更にも自動的に対応できる。  
  
UIとそのViewModelクラスの間のインタラクションは、主にイベント入力とそれに続く状態出力として認識できるので、その関係は次の図のように表現できる。  
  
<img width="529" alt="スクリーンショット 2024-07-21 18 49 30" src="https://github.com/user-attachments/assets/ec05d3ee-4474-4dac-85ba-fe132dd11a81">

状態が下に流れ、イベントが上に流れるパターンを、単方向データフロー（UDF）と呼ぶ。このパターンは、アプリアーキテクチャにおいて次のことを意味する。  
- ViewModelは、UIが使用する状態を保持し公開する。UI状態は、ViewModelによって変換されたアプリデータである。
- UIは、ユーザーイベントをViewModelに通知する。
- ViewModelは、ユーザーアクションを処理し、状態を更新する。
- 更新された状態は、UIにフィードバックされてレンダリングされる。
- 上記のステップは、状態の変化を引き起こす任意のイベントで繰り返される。  
  
ナビゲーションデスティネーションまたは画面の場合、ViewModel はリポジトリまたはユースケースクラスと連携し、データを取得してUI状態に変換するとともに、状態の変化を引き起こす可能性があるイベントの結果を取り込む。 
例えば、ユーザーによる記事のブックマークのリクエストは、状態の変化を引き起こすイベントの一例である。UIがレンダリングを完全に行えるよう、UI状態のすべてのフィールドに値を入力するロジック、およびイベントを処理するロジックをもれなく定義することが、状態プロデューサとしてのViewModelの役割である。  

<img width="636" alt="スクリーンショット 2024-07-21 19 02 59" src="https://github.com/user-attachments/assets/6b1782fb-95bb-4807-a208-f76e9f2dcffd">

#### ロジックのタイプ
- `ビジネスロジック`は、アプリデータに関するプロダクトの要件の実装である。通常、ビジネスロジックはドメインレイヤまたはデータレイヤに配置され、UIレイヤに配置されることはない。
- `UIロジック`は、状態の変化を画面に表示する「方法」を意味する。たとえば、`Android Resources`を使って画面に表示する適切なテキストを取得する、ユーザーがボタンをクリックしたときに特定の画面に移動する、`トースト`または`スナックバー`を使ってユーザーメッセージを画面に表示するなどの方法がある。

UIロジックは、特に`Context`のようなUIタイプを含む場合、ViewModelではなくUIで実行する必要がある。UIが複雑になるため、テストのしやすさと関心の分離を重視してUIロジックを別のクラスに委任したい場合は、単純なクラスを状態ホルダーとして作成することができる。UIに作成される単純なクラスは、UIのライフサイクルに従うので、Android SDKの依存関係を利用できる。

#### UDFを使用する理由
- `データの整合性` UIに対して、信頼できる唯一のデータソースが存在する
- `テストのしやすさ` 状態のソースが分離されるため、UIから独立してテストを行うことができる
- `メンテナンスのしやすさ` 状態の変化は、ユーザーイベントおよびデータソースからのデータ取得の結果であるという、明確に定義されたパターンに従う

## UI状態を公開する
UI状態を定義し、その状態の生成を管理する方法を決定したら、次のステップとして、生成された状態をUIに公開する。UDFを使用して状態の生成を管理するので、生成される状態をストリームとみなすことができる。つまり、時間の経過とともに状態の複数のバージョンが生成される。したがって、LiveDataやStateFlowなどの監視可能なデータホルダーでUI状態を公開する必要がある。これは、UIが、ViewModelから直接手動でデータを取得する手間をかけずに、状態の変更に反応できるようにするためである。このようなタイプには、常にUI状態の最新バージョンがキャッシュに保存されるというメリットもあり、構成変更後に状態をすばやく復元するためにも役立つ。  

UiStateのストリームを作成する一般的な方法は、可変のバッキングストリームをViewModelからの不変のストリームとして公開することである。たとえば、`MutableStateFlow<UiState>`を`StateFlow<UiState>`として公開する。
```Kotlin
class NewsViewModel(
    private val repository: NewsRepository,
    ...
) : ViewModel() {

    private val _uiState = MutableStateFlow(NewsUiState())
    val uiState: StateFlow<NewsUiState> = _uiState.asStateFlow()

    private var fetchJob: Job? = null

    fun fetchArticles(category: String) {
        fetchJob?.cancel()
        fetchJob = viewModelScope.launch {
            try {
                val newsItems = repository.newsItemsForCategory(category)
                _uiState.update {
                    it.copy(newsItems = newsItems)
                }
            } catch (ioe: IOException) {
                // Handle the error and notify the UI when appropriate.
                _uiState.update {
                    val messages = getMessagesFromThrowable(ioe)
                    it.copy(userMessages = messages)
                 }
            }
        }
    }
}
```
#### その他の考慮事項
- **1つのUI状態オブジェクトで、互いに関連付けられた複数の状態を処理するようにする**  
そうすることで、データの不整合が減少し、コードが理解しやすくなる。ニュースアイテムのリストとブックマーク数を2つの異なるストリームで公開すると、一方が更新され、もう一方が更新されない状況が生じる可能性がある。使用するストリームを1つにすれば、両方の要素が常に最新の状態に保たれる。さらに、一部のビジネスロジックでは、ソースの組み合わせが必要になる場合がある。たとえば、ブックマークボタンを表示する必要があるのは、ユーザーがログイン済みで、かつプレミアムニュースサービスを定期購読している場合のみとする。その場合、次のようにUI状態クラスを定義できる。
```Kotlin
data class NewsUiState(
    val isSignedIn: Boolean = false,
    val isPremium: Boolean = false,
    val newsItems: List<NewsItemUiState> = listOf()
)

val NewsUiState.canBookmarkNews: Boolean get() = isSignedIn && isPremium
```
ただし、次のように、ViewModelからの個別の状態ストリームを使用することが適切な場合もある。  
- **関連のないデータ型**  
UIのレンダリングに必要な状態は、互いに完全に独立している場合がある。このような場合は、完全に異なる複数の状態を一緒にまとめると、コストがメリットを上回る可能性がある。ある状態が他の状態より頻繁に更新される場合は、特にそうである。
- **UiStateの差分抽出**  
UiStateオブジェクト内のフィールドの数が多いと、その分だけフィールドが更新された結果としてストリームが出力される可能性が高くなる。Viewには、連続する出力が異なるものか同じものかを把握する差分抽出メカニズムがないため、出力ごとにビューの更新が発生する。つまり、LiveDataで`Flow API`や`distinctUntilChanged()`などのメソッドを使用した緩和策が必要になる場合がある。

### UI状態を使用する
UIでUiStateオブジェクトのストリームを使用するには、アプリで使用している監視可能なデータ型に終端演算子を使用する。たとえば、LiveDataの場合は`observe()`メソッドを使用し、Kotlin Flowの場合は`collect()`メソッドまたはそのバリエーションを使用する。   
UIで監視可能なデータホルダーを使用する際は、必ずUIのライフサイクルを考慮する必要がある。これが重要なのは、Viewがユーザーに表示されていないとき、UIはUI状態を監視すべきでないからである。LiveDataを使用している場合は、`LifecycleOwner`が暗黙的にライフサイクルに関する問題に対処する。Flowを使用している場合は、適切なコルーチンスコープと`repeatOnLifecycle API`を使用してこの問題を処理することをおすすめする。
```Kotlin
class NewsActivity : AppCompatActivity() {

    private val viewModel: NewsViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        ...

        lifecycleScope.launch {
            repeatOnLifecycle(Lifecycle.State.STARTED) {
                viewModel.uiState.collect {
                    // Update UI elements
                }
            }
        }
    }
}
```
Composeの場合は以下のようになる
```Kotlin
@Composable
fun LatestNewsScreen(
    modifier: Modifier = Modifier,
    viewModel: NewsViewModel = viewModel()
) {
    Box(modifier.fillMaxSize()) {

        if (viewModel.uiState.isFetchingArticles) {
            CircularProgressIndicator(Modifier.align(Alignment.Center))
        }

        // Add other UI elements. For example, the list.
    }
}
```
#### 画面にエラーを表示する
たとえば、記事のフェッチ中にエラーが発生した場合、通常は、ユーザーに問題の詳細を示す1つ以上のメッセージを表示する。
```Kotlin
data class Message(val id: Long, val message: String)

data class NewsUiState(
    val userMessages: List<Message> = listOf(),
    ...
)
```
