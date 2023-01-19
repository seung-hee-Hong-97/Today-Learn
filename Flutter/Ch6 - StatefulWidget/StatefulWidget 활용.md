# StatefulWidget 활용

<br />

## StatefulWidget의 특징

- State 객체를 갖는 위젯
- State 객체의 `setState` 메서드가 위젯의 상태 변화를 알려줌
- 클라이언트의 조작으로 `setState` 메서드가 호출되면 Flutter가 위젯을 다시 그림

<br />

## 기본 형태

StatelessWidget을 자동완성 한 것과 비슷하게 stf을 입력 후 Enter를 치면 자동완성 됩니다.

``` dart
// class 이름 입력 전
class  extends StatefulWidget {
  const ({Key? key}) : super(key: key);

  @override
  _State createState() => _State();
}

class _State extends State<> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

// class 이름 입력 후
class Example extends StatefulWidget {
  const Example({Key? key}) : super(key: key);

  @override
  _ExampleState createState() => _ExampleState();
}

class _ExampleState extends State<Example> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

> class 이름을 입력하면 자동으로 Constructor, State의 이름도 입력됩니다.

<br />

**StatefulWidget**을 상속받는 위젯이 **createState** 메서드로 **State** 객체를 리턴하고, **State**를 상속받는 객체가 **build** 메서드로 **Widget**을 리턴하는 형태입니다.

이 때 State 객체 이름 앞에 자동으로 **언더바(_)**가 붙는데, Dart에서 클래스, 프로퍼티, 메서드 앞에 언더바를 붙이면 **private**이라는 의미입나다.

클래스가 private일 경우 해당 파일에서만 접근할 수 있고, 프로퍼티와 메서드는 해당 클래스에서만 접근할 수 있습니다.

<br />

## setState

Flutter에 **state값이 변경** 되었다고 알려 **build 메서드를 다시 호출** 하도록 하는 메서드입니다.

비동기 코드를 실행할 수 없기 때문에 setState 메서드 실행 전에 모든 비동기 코드를 완료해야 합니다.

> 자동 완성된 StatefulWidget에는 setState 메서드가 없습니다.
>
> 따라서 setState 메서드를 별도로 선언하여 사용해야 합니다.

<br />

## 사용 예제

박스를 클릭하면 숫자가 증가하는 예제를 만들어보겠습니다.

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
  int number = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GestureDetector(
        onTap: () {
          setState(() {
            number++;
          });
        },
        child: Center(
          child: Container(
            width: 100,
            height: 100,
            color: Colors.blue,
            child: Center(
              child: Text(
                number.toString(),
                style: TextStyle(
                  color: Colors.white,
                  fontSize: 38,
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

<img src="https://user-images.githubusercontent.com/68320595/213125625-02a8692f-c778-4a7a-bfc7-9af16b27fa9c.gif" height="400" />
