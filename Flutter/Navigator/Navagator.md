# Navigator

화면 이동시 사용되는 객체입니다. 

push, pop 을 사용하여 Navigation Stack을 통해 이동이 가능합니다. 

<br />

## push

기본적인 페이지 이동입니다. 

```dart
 Navigator.of(context).push(MaterialPageRoute(builder: (_) => SettingScreen());
```

<br />

## pushReplacement

이동 시 현제 stack을 자신으로 교체합니다. == 호출하는 페이지의 스택을 자신으로 교채합니다.

```dart
Navigator.of(context).pushReplacement(MaterialPageRoute(builder: (_) => SettingScreen()));
```

<br />

## pushAndRemoveUntil

push동작의 실행과 동시에 Navigation Stack을 조절할 수 있습니다. 

2번째 인자를 사용하여 stack에 filter 를 적용하여 stack값을 조절합니다.

```dart
// 일괄 적용 (false -> Navigation Stack 모두 제거, true -> 일반적인 Push와 동일)
Navigator.of(context).pushAndRemoveUntil(MaterialPageRoute(builder: (_)=>SettingScreen()),
                            (route) => false);

// 조건식 (route.setting.name에 만족하는 NamedRoute 빼고 모두 Navigation Stack에서 제거)
Navigator.of(context).pushAndRemoveUntil(MaterialPageRoute(builder: (_)=>SettingScreen()),
                            (route) => route.settings.name == '/');
```

<br />

## pop

뒤로가기 동작입니다. 이전 페이지로 이동합니다. stack이 없는 경우에도 실행되며 Navigaton Stack에 페이지가 없기 때문에 빈 화면이 나타납니다.

```dart
Navigator.of(context).pop();
```

<br />

## maybePop

navigation stack이 없을 시 동작하지 않습니다. pop이 가능할 때 동작합니다.

```dart
Navigator.of(context).maybePop();
```

<br />

## canPop

뒤로가기 기능 여부 확인할 때 사용합니다. Navigation Stack의 존재 여부를 확인는 것과 동일한 의미

``` dart
Navigator.of(context).canPop();

// Navigation Stack이 존재할 경우 - true
// Navigation Stack이 존재하지 않을 경우 - false
```

<br />

## Android 뒤로가기 버튼 제어

`WillPopScope` 위젯의 `onWillPop` 속성을 통해 뒤로가기 여부를 정할 수 있습니다.

```dart
WillPopScope( // pop 동작에 대한 이벤트 제어 Scope
      onWillPop: () async {
        // true / false <- 뒤로가기 가능/불가능 여부
        // 하드웨어 백버튼을 통한 뒤로가기를 제어할 수 있다.
        // 명시적으로 pop 동작 수행 시 해당 조건을 무시하고 실행됩니다.
        return Navigator.of(context).canPop();
      },
      child: MaterialApp(
        initialRoute: '/',
        routes: {
          '/': (_) => RandomNumber(),
          '/frame': (_) => FrameHome(),
          '/datetime': (_) => DateTimeSample(),
        },
      ),
    );
```

<br />

## 호출 시 데이터 전달

기본적으로 생성자를 통한 호출이 이루어지기에 생성자를 통해 인자를 전달합니다. 

```dart
class SettingScreen extends StatefulWidget {
  final ParamType param; // 받을 인자 몀시
  const SettingScreen({Key? key, requried this.param}) : super(key: key);

  @override
  State<SettingScreen> createState() => _SettingScreenState();
}

...
```

argument를 통해 데이터를 가져오는 방법 또한 있습니다. 

```dart
// RouteSettings를 통한 값 전달 
Navigator.of(context).push<int>(
    MaterialPageRoute(
        builder: (_) => SettingScreen(),
        settings: RouteSettings(
               arguments: 200
          )
));

// ModalRoute 를 통하여 받는 부분
final arguments = ModalRoute.of(context)!.settings.arguments;
```

<br />

## Route를 사용하여 페이지 이동 및 값 전달

nameRoute를 사용하여 페이지 이동 및 argument를 사용할 수 있다.

```dart
// 
MaterialApp(
      initialRoute: '/', // 시작 페이지
      routes: {
        '/': (_) => RandomNumber(),
        '/frame': (_) => FrameHome(),
        '/datetime': (_) => DateTimeSample(),
      },
 );


// 페이지 이동
Navigator.of(context).pushNamed('/frame', arguments: 100);
```

<br />

## 데이터 반환  

navigotor로 페이지를 전환하며 값을 주고받는 방법에 대하여 설명합니다. 

```dart
// 페이지 호출 
Navigator.of(context).push(MaterialPageRoute(builder: (_) async { 
    
    var result = await SettingScreen(); 
    ...
});
                           
// 페이지 호출 반환 타입 지정 (선택사항)
Navigator.of(context).push<int>(MaterialPageRoute(builder: (_) async { 
    
    int result = await SettingScreen(); 
    ...
});
                                                
// 뒤로가기 
Navigator.of(context).pop(returnValue);
```
