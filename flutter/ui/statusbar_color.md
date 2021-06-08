## Status Barの背景色変更方法
```Dart
import 'package:flutter/services.dart';

SystemChrome.setSystemUIOverlayStyle(
      SystemUiOverlayStyle(
        statusBarColor: Colors.transparent,
      )
  );
```  
## Status Barのテキスト色の変更方法

- `Brightness.light` 文字色は黒
- `Brightness.dark` 文字色は白 

```Dart
theme: ThemeData(
  appBarTheme: AppBarTheme(
    brightness: Brightness.light
  )
)
```
