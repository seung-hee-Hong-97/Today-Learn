# 기초 위젯

Flutter에는 많은 Widget 들이 많지만 가장 많이 사용되는 UI를 구성하는 4가지의 Widget을 알아 보겠습니다.

<br/>

## Text

텍스트를 렌더링 하는 위젯입니다.

``` dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}): super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Text('Hello')
    );
  }
}
```

> 텍스트를 넣을 땐 `Text('텍스트 내용')` 형태로 넣습니다.

<img src="https://user-images.githubusercontent.com/68320595/212278169-361f6e8e-459a-437b-88ed-b0b1f5d46486.png" />

<br />

## Icon

아이콘을 렌더링 하는 위젯입니다.

``` dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}): super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Icon(Icons.star)
    );
  }
}
```

> 아이콘을 넣을 땐 `Icon(Icons.아이콘이름)`  형태로 넣습니다.
>
> 아이콘의 종류는 [Flutter 아이콘 문서](https://api.flutter.dev/flutter/material/Icons-class.html)에 있습니다.

<img src="https://user-images.githubusercontent.com/68320595/212278734-494921ca-8cc3-46ed-ba14-19189cd0f470.png" />

<br/>

## Image

이미지를 렌더링하는 위젯입니다.

``` dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}): super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Image.asset('typescript.png')
    );
  }
}
```

> 이미지를 넣을 땐 `Image.asset('이미지 경로')` 형태로 넣습니다.

<img src="https://user-images.githubusercontent.com/68320595/212282200-2ab38d31-2b47-4eb4-9fde-21e15f178913.png" width="200" />

<br />

## Flutter에서 이미지 사용할 수 있도록 설정 추가

이미지 위젯을 사용하려면 몇가지 설정해줘야 하는 것들이 있습니다.

<br />

### 1. 이미지를 저장할 폴더 생성

현재 Flutter 프로젝트에 이미지 파일을 저장할 폴더를 생성해줘야 합니다.

폴더명은 `assets` 으로 하고 폴더 위치는 Flutter 프로젝트 하위에 생성합니다. -> android, ios와 같은 위치

<br />

### 2. pubspec.yaml에 이미지 폴더 추가

디렉토리 구조를 보면 `pubspec.yaml` 파일이 있을 것 입니다.

해당 파일을 열면 많은 내용이 있을 것인데, 55라인에 있는 flutter 하위에 아래와 같이 추가해줍니다.

``` yaml
flutter:
  assets:
    - assets/
```

> 이미지 폴더를 추가

<br />

## Container

네모박스를 렌더링 하는 위젯입니다.

``` dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}): super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Center(
        child: Container(width: 50, height: 50, color: Colors.red)
      )
    );
  }
}
```

> 네모박스를 만들 땐 `Container(박스 스타일 값)` 형태로 넣습니다.

추가적으로 몇 가지를 더 살펴 보겠습니다.

1. `Center` 위젯은 자식 위젯을 화면 정중앙에 위치시킬 때 사용합니다.
2. 자식 위젯을 선언할 땐 위의 코드처럼 부모 위젯 안에 `child: ` 를 선언하고 자식 위젯을 작성합니다.
3. 색상을 부여하고 싶을 땐 `Colors.색상` 형태로 Colors 클래스를 사용해서 부여합니다.

<img src="https://user-images.githubusercontent.com/68320595/212286620-cf050bab-6af8-4bbb-baf3-6ccde0926845.png" />