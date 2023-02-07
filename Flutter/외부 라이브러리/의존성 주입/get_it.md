# get_it

외부에서 정의된 의존성을 주입 해 주는 라이브러리이다. 

pub_dev링크

https://pub.dev/packages/get_it



singleton, factory, lazy 등 다양한 형태로 의존성을 주입할 수 있도록 보조한다. 

```dart
// 선언 
final LocalDatabase database = LocalDatabase();
GetIt.I.registerSingleton<LocalDatabase>(database);

// 반환
GetIt.I<LocalDatabase>()
```



한번 등록 시킨 의존성을 앱 전역에서 호출 가능하다.

사용을 위해서는 반드시 선언을 먼저 해 주어야 한다. 