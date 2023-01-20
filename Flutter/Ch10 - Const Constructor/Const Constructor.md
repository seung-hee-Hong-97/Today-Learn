# Const Constructor

`const` 생성자를 사용하는 이유는 바로 최적화 때문입니다.

컴파일러는 모든 `const` 객체에 대해 동일한 메모리 부분을 할당하여 객체를 불변으로 만듭니다.

즉 앱을 실행할 때 한 번만 생성한다는 것인데 이 말은 리소스 낭비를 하지 않는 다는 것과 일맥상통합니다.

<br />

`const`와 가장 많이 비교되는 것이 `final`이 있습니다. 

- 공통점으로는 `한 번 설정한 값을 변경할 수 없다, 다른 값으로 변경하려고 시도하면 컴파일 오류가 발생한다` 
- 차이점으로는 `const의 경우 컴파일 타임에서 정의를 하고, 런타임에서는 정의할 수 없다`

차이점을 가장 직관적으로 보여주는 예시가 바로 `DateTime.now()` 로 정의되는 값에 final과 const를 사용해보는 것입니다.

`DateTime.now()`는 런타임에 값이 결정되기 때문에 const를 사용할 수 없습니다.

<br />

## const를 사용하기 적절한 경우

프로젝트를 하다보면 코드 내에서 `setState()`를 사용할 경우가 굉장히 많아집니다.

이 때 `setState()`를 실행하면 범위에 해당하는 모든 변수들이 재성성되는데 이 말은 리소스를 많이 잡아먹는다는 의미입니다.

`Text()` 와 같은 정해진 값일 경우 매번 재생성할 필요가 없기 때문에 const를 사용하여 자원을 낭비하지 않도록 해주는 것이 좋습니다.

<br />

## 사용 예시





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
      body: Container(
        width: MediaQuery.of(context).size.width,
        child: Column(
          children: [
            const TestWidget(label: 'Test1'),  // setState가 실행되어도 재실행 X
            TestWidget(label: 'Test2'),  // setState가 실행되면 재실행 O
            ElevatedButton(
              onPressed: () {
                setState(() {});  
              },
              child: Text('child'),
            ),
          ],
        ),
      ),
    );
  }
}

// 테스트 위젯 생성
class TestWidget extends StatelessWidget {
  final String label;
  
  const TestWidget({required this.label, Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    print('$label: Build 실행');
    
    return Container(
      child: Text(label),
    );
  }
}
```

> const가 붙은 `TestWidget(label: Test1)`은 버튼을 클릭하여 `setState()`가 실행되어도 Test1이 출력되지 않는 것을 확인할 수 있습니다.