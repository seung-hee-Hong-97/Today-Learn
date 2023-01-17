# Splash Screen 구현

앱을 실행할 때 최초로 보이는 화면을 `Splash Screen` 이라고 부르는데, Splash Screen을 만드는 방법을 예제를 통해 알아 보도록 하겠습니다.

<br />

## 필수 정보

Splash Screen 구현을 위해 필요한 지식들이 몇 가지 있는데 아래와 같습니다.

- Asset 추가하기
- StatelessWidget 생성하기
- Column 위젯
- CircularProgressIndicator 위젯
- Image 위젯

<br />

## 1. asset 폴더 추가하기

이미지 파일을 관리 할 폴더인 `asset` 폴더를 프로젝트 최상위에 생성합니다.

<img src="https://user-images.githubusercontent.com/68320595/212633708-eae55415-d13b-4a1f-b8dd-1c81810914f0.png" />

> `asset` 폴더 하위에 이미지 파일만 관리할꺼면 상관 없지만, 그게 아닌 아이콘 등 다른 파일도 관리할꺼면 이미지 파일만 관리 할 `img` 폴더를 추가적으로 생성 하는 것이 가독성도 좋고 파일 관리하기가 더 편할 것 입니다.

<br />

## 2. pubspec.yaml 파일 설정

`pubspec.yaml` 파일을 열어 `The following section is specific to Flutter packages.` 문구가 주석처리 되어 있는 부분 하단에 아래와 같이 설정해줍니다.

``` yaml
flutter:
  assets:
    - asset/img/
```

> 이미지 파일이 저장 될 경로를 pubspec.yaml 파일에 설정하는 것입니다.
>
> 추가적으로 위와 같이 설정을 했다면 꼭 `pub get` 을 클릭하여 yaml 파일을 반영해줘야 합니다.

<img src="https://user-images.githubusercontent.com/68320595/212634757-3bdb3690-28ae-4ea7-8fb8-1cda33416187.png" />

<br />

## 3. main.dart 기본 설정

Flutter 프로젝트를 생성하고 `lib > main.dart` 파일을 열게 되면 `main` 함수 하위에 기본으로 설정된 여러 코드가 있을 것입니다.

main 함수를 제외한 모든 코드는 제거해주고 아래와 같이 코드를 작성해줍니다.

``` dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        body: // ... 코드 작성
      ),
    ),
  );
}
```

<br />

간단하게 화면 정중앙에 `Hello World` 문구를 띄울 수 있도록 코드 작성을 해보겠습니다.

``` dart
void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        body: Center(
          child: Text("Hello World"),
        ),
      ),
    ),
  );
}
```

> `Center`: 자식 위젯을 화면 정중앙에 위치하도록 하는 위젯
>
> `child` : 자식 위젯을 작성하는 Center 위젯의 속성
>
> `Text` : 텍스트를 렌더링할 때 필요한 위젯

<img src="https://user-images.githubusercontent.com/68320595/212641004-e035292b-a793-44d3-b9f5-52cf2c8228cf.png" />

<br />

그런데 만일 코드가 길어질 경우 위와 같은 방식으로 코딩을 하게 되면 들여쓰기도 매우 깊어질 것이고,

가독성도 좋지 않을 것입니다.

따라서 `Scaffold` 를 클래스로 분리하여 리턴해주는 방식으로도 구현할 수 있습니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

// StatelessWidget을 상속받아서 HomeScreen에서 사용
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('Hello World'),
      ),
    );
  }
}

/*
  아마 class HomeScreen extends StatelessWidget을 입력하게 되면, HomeScreen에 빨간 밑줄이 보일 것입니다.
  이건 build를 override 하지 않아서 생기는 오류인데, HomeScreen을 클릭하면 빨간 전구 아이콘이 보입니다.
  아이콘을 클릭하여 Create 1 miss override를 클릭하면 기본적으로 build 코드 블럭이 자동생성됩니다.
*/
```

<br />

## 4. Splash 이미지 적용시키기

지금까지는 글자를 렌더링하기 위해 `Text` 위젯을 사용해봤는데 이미지를 렌더링 하기 위해선 `Image` 위젯을 사용해야 합니다.

코드를 이어서 작성하자면 `Text('Hello World')` 부분을 Image 위젯으로 바꿔주기만 하면 됩니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

// StatelessWidget을 상속받아서 HomeScreen에서 사용
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Image.asset('asset/img/splash.png'),
      ),
    );
  }
}
```

> 이미지 위젯 사용법은 `Image.asset('이미지 경로')` 입니다.
>
> 여기서 주의할 점은 이미지 경로는 최상위부터 풀 경로로 적어줘야 합니다.
>
> 또한 이미지가 렌더링 되지 않고 `unable to load image asset` 오류가 발생하면 다음과 같은 과정을 통해 해결할 수 있습니다.
>
> `실행 중인 앱 종료 > Android Studio 종료 후 재 시작 > 다시 앱 실행` 

<br />

## 5. 로딩 스피너 추가하기

스플래시 이미지를 띄우고, 그 밑에 로딩 스피너를 띄우고 싶은데 여기서 문제가 있습니다.

`Center` 위젯은 자식 위젯으로 하나 밖에 선언할 수 없지만 Splash 이미지와 로딩 스피너를 적용시켜려면 복수의 위젯이 필요합니다.

따라서 `Column` 위젯을 사용하여 구현할 것 입니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Color(0xFF9c14fc),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,  // 세로를 기준축으로 가운데 정렬
        children: [
          Image.asset('asset/img/splash.png'),
          CircularProgressIndicator(
            color: Colors.white,
          ),
        ],
      ),
    );
  }
}
```

> `CircularProgressIndicator` : 로딩 스피터를 보여주는 위젯이며 괄호 안에 색상을 넣을 수 있습니다.
>
> `HexCode 색상` : HexCode는 16진수 이기 때문에 0x로 시작해야 하고, FF를 붙혀 투명도를 없애줘야 합니다.
>
> `mainAxisAlignment` : Column / Row의 기준축을 의미하는 속성입니다. (기준축: Column -세로, Row - 가로)

<img src="https://user-images.githubusercontent.com/68320595/212790598-3cd8fb57-ea06-4b9a-8374-ba382b0f8152.gif" height="500" />

