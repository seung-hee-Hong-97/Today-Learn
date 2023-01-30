# Modal Bottom Sheet

사용자가 버튼을 클릭하면 뒤에 있는 내용을 가리는 하단 시트를 표시하는데 사용합니다.

어떠한 설명을 표시하거나 유저에게 옵션을 제시할 때 해당 위젯을 사용합니다.

<br />

## showModalBottomSheet()

해당 함수가 실행됐을 때 모달이 화면 아래에서 위로 올라옵니다.

<br />

### 2가지 필수 속성

- `BuildContext` : 가져올 위젯을 결정하고 위젯 트리에서 가져올 위젯의 위치를 결정함
- `WidgetBuilder` : 위젯을 반환

```dart
showModalBottomSheet(
  context: context,
  builder: (BuildContext context) {
    // ..
  },
);
```



<br />

## 사용 예시

``` dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: TestScreen(),
  ));
}

class TestScreen extends StatefulWidget {
  const TestScreen({Key? key}) : super(key: key);

  @override
  State<TestScreen> createState() => _TestScreenState();
}

class _TestScreenState extends State<TestScreen> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      floatingActionButton: renderFloatingActionButton(),
      body: Center(
        child: Text('Home Screen'),
      ),
    );
  }

  // FloatingButton 생성
  FloatingActionButton renderFloatingActionButton() {
    return FloatingActionButton(
      onPressed: () {
        // 버튼 클릭시 bottomSheet 열림
        showModalBottomSheet(
          isScrollControlled: true,  // true로 설정 시 스크롤링 부분을 컨트롤 함
          context: context,
          builder: (BuildContext context) {
            return TestBottomSheet();
          },
        );
      },
      child: const Icon(Icons.add),
    );
  }
}

class TestBottomSheet extends StatelessWidget {
  const TestBottomSheet({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // 시스템적 요소(ex - 키보드)로 인해 발생한 영역 만큼의 값 (double 타입)
    final bottomInset = MediaQuery.of(context).viewInsets.bottom;

    return Container(
      color: Colors.white,
      height: MediaQuery.of(context).size.height / 2 + bottomInset,
      child: Padding(
        padding: EdgeInsets.only(bottom: bottomInset),
        child: Column(
          children: [
            TextField(),
          ],
        ),
      ),
    );
  }
}
```

> TextFied 위젯 사용시 키보드 때문에 모달 화면이 짤리는 부분이 있는데, 위와 같이 구현하게 되면 키보드 높이 만큼 Padding을 추가하여 ModalSheet의 영역도 그대로 유지되는 것을 확인할 수 있습니다.

<br />

## 결과

<img src="https://user-images.githubusercontent.com/68320595/215379445-c5af0f50-9c1e-4b5c-817f-409f6a305b71.gif" height="450" />