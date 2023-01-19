# (1) Asset 설정 및 UI 구현

입력한 날짜가 현재 날짜로부터 얼마나 지났는지 혹은 입력한 날짜가 현재 날짜로부터 얼마나 남았는지를 보여주는 앱을 구현해보도록 하겠습니다.

<br />

## 주요 기술

- Font 적용
- DatePicker 사용
- 날짜 (DateTime) 사용
- 테마 적용

<br />

## 1. Asset 추가 및 설정

앱을 구현하기 위해 필요한 이미지, 폰트를 적용을 해야 합니다.

이미지를 적용하는 방법은 많이 다뤘으니 해당 문서에서는 폰트를 적용하는 방법만 다뤄보겠습니다.

우선 [Google Font](https://fonts.google.com/)에 들어가셔서 원하는 폰트를 다운 받아 아래의 이미지와 같이 폴더 구조를 형성합니다.

<img src="https://user-images.githubusercontent.com/68320595/213359159-54a64d65-abbd-4209-9532-8b78b5797e18.png" />

<br />

이미지 경로를 설정할 때와 마찬가지로 `pubspec.yaml` 파일을 열어 경로를 설정해줍니다.

``` yaml
# The following section is specific to Flutter packages.
flutter:
  # ~~~~ 
  assets:
    - asset/img/
    
  fonts:
    - family: parisienne
      fonts:
        - asset: asset/font/Parisienne-Regular.ttf
        
    - family: sunflower
      fonts:
        - asset: asset/font/Sunflower-Light.ttf
        - asset: asset/font/Sunflower-medium.ttf
          weight: 500
        - asset: asset/font/Sunflower-Bold.ttf
          weight: 700
```

> Font 경로를 설정해준 위치에서 조금 내려보면 적용하는 방법이 주석처리 되어 있으니 참고하시면 좋습니다.

<br />

## 2. TextStyle 설정

 `Text` 위젯에 `style` 속성을 통해 스타일링을 할 수 있습니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Text(
        'Hello World!',
        style: TextStyle(
          color: Colors.white,
          fontSize: 30,
          fontFamily: 'sunflower',
          fontWeight: FontWeight.w700,
        ),
      ),
    ),
  ));
}
```

> 위와 같은 형태로 텍스트에 스타일링을 합니다.

<br />

## 3. UI 구현

D-Day 앱을 만들 것이기 때문에 UI에 들어갈만 한 것으로 스타일링 했습니다.

최상위 위젯에 UI 관련 코드가 너무 많으면 가독성이 떨어지고 유지보수할 때 좋지 않으므로 영역 별로 위젯을 만들어서 구현 했습니다.

``` dart
import 'package:flutter/material.dart';

class HomeScreen extends StatefulWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  _HomeScreenState createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.brown[100],
      body: SafeArea(
        child: Container(
          width: MediaQuery.of(context).size.width,
          child: Column(
            children: [
              _TopArea(),
              _BottomArea(),
            ],
          ),
        ),
      ),
    );
  }
}

class _TopArea extends StatelessWidget {
  const _TopArea({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Expanded(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          Text(
            'Pass the Test',
            style: TextStyle(
              color: Colors.white,
              fontFamily: 'sunflower',
              fontSize: 50,
            ),
          ),
          Column(
            children: [
              Text(
                '시험 날짜',
                style: TextStyle(
                  color: Colors.white,
                  fontFamily: 'sunflower',
                  fontSize: 30,
                ),
              ),
              Text(
                '2020-12-12',
                style: TextStyle(
                  color: Colors.white,
                  fontFamily: 'sunflower',
                  fontSize: 20,
                ),
              ),
            ],
          ),
          IconButton(
            iconSize: 50,
            onPressed: () {},
            icon: Icon(Icons.calendar_today),
          ),
          Text(
            'D-3',
            style: TextStyle(
              color: Colors.white,
              fontSize: 60,
              fontFamily: 'sunflower',
            ),
          ),
        ],
      ),
    );
  }
}

class _BottomArea extends StatelessWidget {
  const _BottomArea({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Expanded(
      child: Image.asset('asset/img/test_image.png'),
    );
  }
}
```

<img src="https://user-images.githubusercontent.com/68320595/213386311-3a67924c-b752-4a72-ab5a-f04acda56a4c.png" height="450" />