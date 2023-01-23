# (3) Navigator, Slider 위젯, 데이터 전달

랜덤숫자 생성기 화면에서 다른 화면으로 이동하는 법과 Slider 위젯을 사용하여 특정 범위 안에 있는 값을 선택하는 법을 학습 해보겠습니다.

<br />

## Navigator 이론

화면 간의 이동을 할 때 사용되는 객체입니다.

`push`, `pop` 을 사용하여 페이지 이동을 합니다.

최소 2개의 페이지가 존재해야 Navigator를 사용할 수 있습니다.

<br />

### push

기본적인 페이지 이동 방법 입니다.

```dart
Navigator.of(context).push(MaterialPageRoute(builder: (_) => SettingScreen());
```

<br />

### pop

페이지 뒤로가기 입니다.

``` dart
Navigator.of(context).pop();
```

<br />

### 데이터 반환

navigator로 페이지 전환 간에 데이터를 주고 받는 방법입니다.

``` dart
// 페이지 호출 
Navigator.of(context).push(MaterialPageRoute(builder: (_) async { 
    
    var result = await SettingScreen(); 
    ...
});
                           
// 페이지 호출 반환 타입 지정 (선택사항)
Navigator.of(context).push<int>(MaterialPageRoute(builder: (_) async { 
    
    var result = await SettingScreen(); 
    ...
});
                                                
// 뒤로가기 
Navigator.of(context).pop(returnValue);
```

<br />

## Navigator 적용

다시 프로젝트로 돌아와서, Header의 아이콘을 클릭 했을 때 `SettingsScreen` 화면으로 이동하는 방법을 구현해보겠습니다.

우선 `settings_screen.dart` 파일을 생성하여 페이지를 생성해줍니다.

그 후 아래의 코드와 같이 `Navigator.of(context).push()` 를 사용하여 페이지 이동을 해줍니다.

``` dart
IconButton(
  onPressed: () {
    Navigator.of(context).push(
      MaterialPageRoute(builder: (BuildContext context) {
        return SettingsScreen();
      }),
    ),
  },
),
```

<br />

## Slider 위젯

`Slider` 위젯은 특정 범위에 있는 값을 선택할 때 사용합니다.

Slider 위젯을 사용하면 연속적인 값 혹은 불연속적인 값을 선택할 수 있습니다.

- `value` : double 타입이고 Slider가 현재 선택하고 있는 값
- `min` : double 타입이고 Slider의 최소 값
- `max` : double 타입이고 Slider의 최대 값
- `label` : String 타입이고 Slider를 드래그할 때 위에 표시할 라벨
- `onChanged` 
  -  `ValueChanged<double>` 타입이고  사용자가 Slider를 드래그해서 새로운값으로 바뀔때 onChanged가 불리는데 이때 현재값을 double로 넘겨줍니다 onChanged에서 받은 값으로 Slider의 value값을 바꿔줘야합니다

<br />

### Slider 사용 예시

``` dart
Slider(
  value: maxNumber,
  min: 100,
  max: 10000,
  onChanged: (double val) {
    setState(() {
      maxNumber = val;
    });
  },
),
```

<br />

## 이전 페이지로 돌아가기 + 데이터 전달

`SettingScreen` 에서 `HomeScreen` 으로 페이지 이동하는 것과 데이터 전달하는 법을 구현해보도록 하겠습니다.

`SettingScreen` 페이지에서 `저장` 버튼을 클릭하면 이전 페이지로 이동 + 데이터 전달이 되는 방식입니다.

``` dart
// 데이터 전달 (SettingScreen)

SizedBox(
  width: MediaQuery.of(context).size.width,
  child: ElevatedButton(
    onPressed: () {
      Navigator.of(context).pop(maxNumber.toInt());  // 이전 페이지로 돌아가고 데이터로 maxNumber 전달
    },
    style: ElevatedButton.styleFrom(
      backgroundColor: RED_COLOR,
    ),
    child: Text('저장!'),
  ),
),
```

<br />

```dart
// 데이터 받음 (HomeScreen)

// async / await를 통해 비동기 처리
void onSettingsPop() async {
  // 전달 받은 데이터가 Null일 수도 있기 때문에 ?를 사용해 Nullabㅣe 변수에 받아줌
  final int? result = await Navigator.of(contet).push(
    MaterialPageRoute(builder: (BuildContext context) {
      return SettingsScreen();
    }),
  );
  
  // Null이 아닐 경우에만 setState()를 통해 maxNumber를 받아온 데이터로 갱신
  if (result != null) {
    setState(() {
      maxNumber = result;
    })
  }
}
```

<br />

## 다음 페이지로 데이터 전달

상위 위젯에서 하위 위젯으로 변수, 콜백함수를 내려주던 방식과 비슷하게

현재 페이지가 상위 위젯, 다음 페이지가 하위 위젯의 개념으로 생각하면 이해하기 쉽습니다.

``` dart
// 현재 페이지 (HomeScreen)
void onSettingsPop() async {
  final int? result = Navigator.of(context).push(
    MaterialPageRoute(builder: (BuildContext context) {
      return SettingsScreen(maxNumber: maxNumber);  // SettingsScreen에 maxNumber를 넘겨줌
    }),
  );
}


// 다음 페이지 (SettingsScreen)
class SettingsScreen extends StatefulWidget {
  // HomeScreen에서 넘겨준 maxNumber를 int 타입으로 받아줌
  final int maxNumber;

  const SettingsScreen({
    required this.maxNumber,
    Key? key,
  }) : super(key: key);

  @override
  _SettingsScreenState createState() => _SettingsScreenState();
}

class _SettingsScreenState extends State<SettingsScreen> {
  double maxNumber = 1000;  // 최초 maxNumber 값은 double 타입의 1000

  @override
  void initState() {
    super.initState();
    
    // State가 생성되고 나서 maxNumber에 받아온 maxNumber 값을 할당
    maxNumber = widget.maxNumber.toDouble();
  }
  
  // ,. otherCode
}
```

<br />

## 중복되는 코드 컴포넌트로 관리

UI를 구현할 때 같은 코드를 여러 번 작성하여 구현하는 경우가 있습니다.

이럴 때 반복되는 코드를 컴포넌트로 빼서 컴포넌트를 호출하는 방식으로 구현하게 되면 반복적으로 사용되는 코드를 줄일 수 있습니다.

예를 들어 아래와 같은 코드가 반복적으로 사용되고 있다고 해보겠습니다.

``` dart
Row(
  children: maxNumber.toString().split('').map(
    (v) => Image.asset('asset/img/$v.png', height: 60),
  ).toList();
),
```

<br />

Row 위젯을 컴포넌트로 빼서 원래 Row 위젯이 있던 위치에는 컴포넌트를 호출하는 방식으로 변경 해보겠습니다.

컴포넌트를 관리하는 디렉토리 경로는 `lib > component > number_row.dart` 로 가정

``` dart
import 'package:flutter/material.dart';

class NumberRow extends StatelessWidget {
  final int number;

  const NumberRow({
    required this.number,
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Row(
      children: number
          .toString()
          .split('')
          .map(
            (e) => Image.asset('asset/img/$e.png', height: 60),
          )
          .toList(),
    );
  }
}
```

> NumberRow 컴포넌트 생성

<br />

`HomeScreen`, `SettingsScreen` 에서 Row 위젯이 사용되던 위치에 아래와 같이 적용

``` dart
NumberRow(number: number 값)
```

