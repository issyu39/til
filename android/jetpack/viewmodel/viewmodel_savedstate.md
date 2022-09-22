# ViewModelによる状態保存

## ViewModelで状態保存できるパターン  
- Configuration Changeや画面回転では状態を保持できる  
- Process Deathでは状態はクリアされてしまうため、必要に応じて`onSaveInstanceState()`を使用する   
  
<img width="491" alt="スクリーンショット 2022-09-21 22 51 19" src="https://user-images.githubusercontent.com/16067422/191522304-317956b6-9260-4951-9367-cea3dfd141d0.png">

## ViewModelのセットアップ
`Fragment 1.2.0`および`Activity 1.1.0`以降であれば、Fragmentから`requireArguments()`で取得しなくてもViewModelで直接`SavedStateHandle`に保存された値を取得できる
```Kotlin
class SavedStateViewModel(private val state: SavedStateHandle) : ViewModel() { ... }

class SearchFragment: Fragment{
  private val vm: SavedStateViewModel by viewModels()
}
```

## SavedStateHandleの値の取得例

### LiveData
```Kotlin
class SavedStateViewModel(private val savedStateHandle: SavedStateHandle) : ViewModel() {
    val filteredData: LiveData<List<String>> =
        savedStateHandle.getLiveData<String>("query").switchMap { query ->
        repository.getFilteredData(query)
    }
    
    // ユーザーの入力値を保持する場合
    val _userTextInput: MutableLiveData<String> = savedStateHandle.getLiveData<String>("USER_TEXT_INPUT", "")
    val userTextInput: LiveData<String> get() = _userTextInput

    fun setQuery(query: String) {
        savedStateHandle["query"] = query
    }
}
```

### StateFlow
```Kotlin
class SavedStateViewModel(private val savedStateHandle: SavedStateHandle) : ViewModel() {
    val filteredData: StateFlow<List<String>> =
        savedStateHandle.getStateFlow<String>("query")
            .flatMapLatest { query ->
                repository.getFilteredData(query)
            }

    fun setQuery(query: String) {
        savedStateHandle["query"] = query
    }
}
```

## ComposeのState
```Kotlin
class SavedStateViewModel(private val savedStateHandle: SavedStateHandle) : ViewModel() {

    val filteredData: List<String> by savedStateHandle.saveable {
        mutableStateOf(emptyList())
    }

    fun setQuery(query: String) {
        withMutableSnapshot {
            filteredData = query
        }
    }
}
```

## 参考
Saved State module for ViewModel   
https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate?hl=ja#savedstatehandle
