# Stack 위젯

Stack 위젯은 자식 요소들을 층층이 쌓을 때 사용합니다.

첫번 째 자식 요소를 맨 밑에 쌓고, 그 다음 자식 요소들을 그 위에 쌓아가는 형태입니다.

자료구조에서 나오는 stack과 동일한 개념이라고 생각하면 됩니다.

<br />

## 사용 예시

``` dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: StackScreen(),
  ));
}


class StackScreen extends StatelessWidget {
  const StackScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Stack(
          children: 
        ),
      ),
    );
  }
}
```

