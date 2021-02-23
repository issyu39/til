## View Binding
モジュール内でView Bindingを有効にすると、そのモジュール内に存在するXMLレイアウトファイルごとにBindingクラスが生成されます。  
Bindingクラスのインスタンスには、対象レイアウト内でIDを持つすべてのViewへの直接参照が組み込まれます。

### セットアップ
```Groovy
android {
        ...
        viewBinding {
            enabled = true
        }
    }
```

### ActivityでView Bindingを使用する
```Kotlin
    private lateinit var binding: ResultProfileBinding

    override fun onCreate(savedInstanceState: Bundle) {
        super.onCreate(savedInstanceState)
        binding = ResultProfileBinding.inflate(layoutInflater)
        val view = binding.root
        setContentView(view)
    }
```    

### FragmentでView Bindingを使用する
```Kotlin
    private var _binding: ResultProfileBinding? = null
    // This property is only valid between onCreateView and
    // onDestroyView.
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = ResultProfileBinding.inflate(inflater, container, false)
        val view = binding.root
        return view
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
    
```    

### Data Bindingとの比較
- **コンパイルが高速:** View Bindingはアノテーション処理を必要としないため、コンパイル時間が短縮されます。 
- **使いやすい:** View Bindingは特別にタグ付けされたXMLレイアウトファイルを必要としないため、より迅速にアプリで利用できます。  
モジュールでView Bindingを有効にすると、そのモジュールのすべてのレイアウトに自動的に適用されます。

逆に、View Bindingには、Data Bindingと比べて次のような制約がある
- View Bindingはレイアウト変数またはレイアウト式に対応していないため、動的UIコンテンツをXMLレイアウトファイルから直接宣言できません。
- View Bindingは双方向データバインディングに対応していません。
