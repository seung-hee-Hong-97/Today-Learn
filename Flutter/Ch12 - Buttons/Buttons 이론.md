# Buttons 이론

Flutter에서 자주 사용되는 버튼들에 대해 다뤄보도록 하겠습니다.

자주 사용되는 버튼은 대략 3가지 정도가 있습니다. `TextButton`, `OutlinedButton`, `ElevatedButton` 

<br />

## TextButotn

텍스트만 보이는 버튼

<br />

## OutlinedButton 

테두리가 있는 버튼

<br />

## ElevatedButton

버튼을 강조할 때 사용

<br />

## Button Styling

버튼에 스타일링을 부여하는 방법은 `styleFrom`, `ButtonStyle` 2가지가 있습니다.

<br />

### styleFrom

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Button'),
      ),
      body: Padding(
        padding: const EdgeInsets.symmetric(horizontal: 8.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            ElevatedButton(
              onPressed: () {},
              child: Text('ElevatedButton'),
              style: ElevatedButton.styleFrom(  // style: 버튼위젯.styleFrom(적용시킬 스타일 작성)
                backgroundColor: Colors.red,
                foregroundColor: Colors.white,
                shadowColor: Colors.green,
                textStyle: TextStyle(
                  fontSize: 20.0,
                ),
                padding: EdgeInsets.all(16.0),
                side: BorderSide(
                  color: Colors.black,
                  width: 4.0,
                ),
              ),
            ),
            OutlinedButton(
              onPressed: () {},
              child: Text('OutlinedButton'),
            ),
            TextButton(
              onPressed: () {},
              child: Text('TextButton'),
            ),
          ],
        ),
      ),
    );
  }
}
```

- `primary` -> `backgroundColor` : 버튼의 배경색
  - `ElevatedButton`에서 primary -> backgroundColor, `OutlinedButton / TextButton`에서 primary -> foregroundColor로 변경 (primary는 3.1.0 버전 이후 부턴 사용할 수 없음)
- `onPrimary` -> `foregroundColor` : 버튼을 클릭 했을 때 배경색 및 버튼의 글자색
  - ElevatedButton에서 onPrimary -> foregroundColor로 변경 (onPrimary는 3.1.0 버전 이후부턴 사용할 수 없음)
- `shadowColor` : 버튼의 그림자 색상
- `elevation` : 3D 입체감의 높이
- `textStyle` : 버튼 텍스트의 스타일링
- `padding` : `EdgeInsets.`를 통해 사용하며 버튼 내부의 Padding 설정
- `side` : `BorderSide(border 스타일링 적용)` 버튼 경계선 스타일링 설정

<br />

### ButtonStyle

`style: ButtonStyle(스타일링 적용)` 형태로 사용합니다.

스타일링 속성을 적용할 때는 `MaterialStateProperty.all()` 혹은 `MaterialStateProperty.resolveWith()` 의 괄호 내부에 적용해야 합니다.

-  `all` : 모든 상황에서 똑같은 스타일링 적용
- `resolveWith` : 내부에 함수를 작성해야 하며, 특정 상태별로 콜백 함수를 받아 유동적으로 스타일링 적용 

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Button'),
      ),
      body: Padding(
        padding: const EdgeInsets.symmetric(horizontal: 8.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            ElevatedButton(
              onPressed: () {},
              child: Text('Button Style'),
              style: ButtonStyle(
                backgroundColor: MaterialStateProperty.all(
                  Colors.black,
                ),
                foregroundColor: MaterialStateProperty.resolveWith(
                  (Set<MaterialState> states) {
                    // 버튼이 눌렸을 때 흰색, 아니면 빨간색
                    if (states.contains(MaterialState.pressed)) {
                      return Colors.white;
                    }
                    
                    return Colors.red;
                  },
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

> ### Material State
>
> - hovered - 마우스를 올렸을 때
> - focused - 포커스 됐을 때 (텍스트 필드)
> - pressed - 눌렸을 때
> - dragged - 드래그 됐을 때
> - selected - 체크박스, 라디오 버튼과 같은 선택 됐을 때
> - scrolledUnder - 다른 컴포넌트 밑으로 스크롤링 됐을 때
> - disabled - 비활성화 됐을 때
> - error - 에러 상태
>
> 실질적으로 버튼에서 사용할만 한 것들은 pressed, disabled 정도가 있습니다.
