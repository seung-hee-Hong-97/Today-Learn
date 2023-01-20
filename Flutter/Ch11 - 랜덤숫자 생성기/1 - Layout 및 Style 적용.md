# (1) Layout 및 Style 적용

<br />

## 주요 기술

- Navigation
- Slider
- Button
- 난수 생성

<br />

## Layout 구성

우선 랜덤숫자 생성기 UI 레이아웃을 배치해보도록 하겠습니다.

``` dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatefulWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  _HomeScreenState createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text('랜덤숫자 생성기'),
                IconButton(
                  onPressed: () {},
                  icon: Icon(Icons.settings),
                ),
              ],
            ),
            Expanded(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text('123'),
                  Text('456'),
                  Text('789'),
                ],
              ),
            ),
            SizedBox(
              width: MediaQuery.of(context).size.width,
              child: ElevatedButton(
                onPressed: () {},
                child: Text('생성하기!'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

<img src="https://user-images.githubusercontent.com/68320595/213623399-afd46ed1-e107-4d31-af75-138c7ae63ebb.png" height="450" />

볼품 없지만 그래도 UI의 기초를 잡았고 이제 스타일링을 해보도록 하겠습니다.

<br />

## 스타일링

스타일링을 하기 전에 쓰이는 색상들을 따로 관리하는 파일을 생성해서 상수를 호출해서 쓰는 방식으로 구현 해보겠습니다.

<img src="https://user-images.githubusercontent.com/68320595/213623677-a64dd283-6a4c-4329-b6ed-c668e42e0010.png" />

위의 이미지와 같이 색상을 관리하는 파일을 생성해줍니다.

``` dart
import 'package:flutter/material.dart';

const Color PRIMARY_COLOR = Color(0xFF2D2D33);
const Color RED_COLOR = Color(0xFFEA4955);
const Color BLUE_COLOR = Color(0xFF549FBF);
```

<br />

이제 본격적으로 스타일링을 해보겠습니다.

``` dart
import 'package:blog_webapp/constant/color.dart';

@override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: PRIMARY_COLOR,  // 앱 배경색 설정
      body: SafeArea(
        child: Padding(  // 화면 전체에 Padding 16px 설정
          padding: const EdgeInsets.all(16.0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  Text(
                    '랜덤숫자 생성기',
                    style: TextStyle(color: Colors.white, fontSize: 30),  // 텍스트 스타일 설정
                  ),
                  IconButton(
                    onPressed: () {},
                    icon: Icon(Icons.settings, color: RED_COLOR),  // 아이콘 스타일 설정
                  ),
                ],
              ),
              Expanded(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    Text('123'),
                    Text('456'),
                    Text('789'),
                  ],
                ),
              ),
              SizedBox(
                width: MediaQuery.of(context).size.width,
                child: ElevatedButton(
                  style: ElevatedButton.styleFrom(  // 버튼 스타일 설정
                    primary: RED_COLOR,
                  ),
                  onPressed: () {},
                  child: Text('생성하기!'),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
```

<img src="https://user-images.githubusercontent.com/68320595/213624590-d834192a-894d-4f4a-af96-d9b1c4ca8859.png" height="450" />

<br />

## map을 활용한 이미지 적용

지금까지 구현된 화면을 보면 잘 보이진 않지만 숫자가 들어있는 영역이 있습니다.

해당 숫자들을 이미지로 바꾸어보도록 하겠습니다.

먼저 이미지를 다운받아서 세팅하도록 하겠습니다.

<Img src="https://user-images.githubusercontent.com/68320595/213629683-83c86e10-a8ed-4c6e-b056-9d454f694566.png" height="300" />

<br />

[전자액자 구현하기](https://github.com/sejong77/Today-Learn/blob/Master/Flutter/Ch7%20-%20%EC%A0%84%EC%9E%90%EC%95%A1%EC%9E%90%20%EA%B5%AC%ED%98%84/%EC%A0%84%EC%9E%90%EC%95%A1%EC%9E%90%20%EA%B5%AC%ED%98%84%ED%95%B4%EB%B3%B4%EA%B8%B0.md) 프로젝트에서도 `map` 메서드를 활용해 여러 이미지를 코드 중복 없이 작성하는 법을 배웠습니다.

이번에는 조금 더 복잡하지만 `map`, `asMap`, `entries` 등을 활용하여 구현해보겠습니다.

``` dart
// 이미지 구현 부분 코드만 작성

Expanded(
  child: Column(
    mainAxisAlignment: MainAxisAlignment.center,
    children: [123, 456, 789].asMap().entries.map(
      // x는 key와 value를 가졌기 때문에 필요한 정보인 인덱스를 얻기 위해선 x.key를 해줘야 함
      (x) => Padding(
        padding: EdgeInsets.only(bottom: x.key == 2 ? 0 : 16.0),
        child: Row(
          // 인덱스가 아닌 123, 456, 789와 같은 값을 얻기 위해선 x.value를 해줘야 함
          children: x.value.toString().split('').map(
            (y) => Image.asset('asset/img/$y.png', height: 60),
          ).toList(),
        ),
      ),
    ).toList(),
  ),
),
```

> `asMap()` : List에 index 키 값을 삽입하여 Map 형태로 반환하는 메서드
>
> `entries` : Map의 key값과 value 값을 매핑하여 꺼낼 수 있는 기능

<img src="https://user-images.githubusercontent.com/68320595/213631383-09c775a3-c15c-4048-b733-857de97adda8.png" height="450" />
