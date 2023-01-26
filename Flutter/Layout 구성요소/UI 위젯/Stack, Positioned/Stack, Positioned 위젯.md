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
          alignment: Alignment.bottomRight,
          children: [
            Container(
              width: 200,
              height: 200,
              color: Colors.red,
            ),
            Container(
              width: 150,
              height: 150,
              color: Colors.blue,
            ),
            Container(
              width: 100,
              height: 100,
              color: Colors.yellow,
            ),
          ],
        ),
      ),
    );
  }
}
```

>### Stack 위젯의 속성
>
>- `alignment` : `AlignmentDirectional.정렬값` 형태로 사용하며 stack의 쌓이는 정렬을 설정
>
>  - AlignmentDirectional의 속성으로는 (topStart, topCenter, topEnd, center, centerStart, centerEnd, bottomStart, bottomEnd, bottomCenter)
>
>  
>
>- `textDecoration` : ltr (왼 -> 오) 또는 rtl (오 -> 왼 )에서 텍스트를 배치
>
>
>
>- `fit` : `StackFit.값` 형태로 사용하며 위치가 지정되지 않은 자식이 스택에 맞게 크기를 조정하는 방식을 결정
>
>  - loose : 하위 위젯 크기를 작게 설정
>  - expand : 하위 위젯의 크기를 최대한 확장
>  - passthrough : 상위 위젯 위치에 따라 상대 위치에 자식 위젯 설정
>
>  
>
>- `clipBehavior` : `Clip.값` 형태로 사용하며 콘텐츠를 클리핑할지 여부를 결정합니다.
>  - none, hardEdge, antiAlias, antiAliasWithSaveLayer가 값으로 있다.

<br />

<img src="https://user-images.githubusercontent.com/68320595/214757706-bed98035-a441-4a03-90bd-3df9679b0b9d.png" height="400" />

> 가장 먼저 작성한 red부터 yellow 순으로 밑에서부터 쌓아가는 형태로 렌더링 된 것을 확인할 수 있습니다.

<br />

# Positioned 위젯

Stack 위젯의 자식 위젯 위치를 설정하는데 쓰입니다.

Positioned 위젯은 Stack 위젯 안에서 사용되어야 하며, `StatelessWidget` 과 `StatefulWidget` 에서만 사용 가능합니다.

`RenderObjectWidget` 과 같은 다른 종류의 위젯에서는 사용할 수 없습니다.

<br />

위의 Stack 위젯 구현 코드에서 Yellow Container에만 Positioned 위젯으로 감싸보겠습니다.

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
          children: [
            Container(
              width: 200,
              height: 200,
              color: Colors.red,
            ),
            Container(
              width: 150,
              height: 150,
              color: Colors.blue,
            ),
            Positioned(
              bottom: 0,
              child: Container(
                width: 100,
                height: 100,
                color: Colors.yellow,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

> ### Positioned 위젯 속성
>
> - left, right, top, bottom, width, height을 줄 수 있고 자식 위젯의 위치를 설정합니다. 
>
> - 타입은 double 입니다.
>
> - Positioned 위젯에는 자식 위젯이 필수적으로 있어야 합니다.

<br />

| bottom: 0                                                    | right: 0                                                     | bottom: 0, right: 0                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="https://user-images.githubusercontent.com/68320595/214758397-1e22540c-3599-4c73-8734-e73905f745e0.png" height="400" /> | <img src="https://user-images.githubusercontent.com/68320595/214758402-f0a661d9-8d5e-4515-8166-8550e8445023.png" height="400" /> | <img src="https://user-images.githubusercontent.com/68320595/214758401-6c3c657e-817f-45f2-85a2-ef754ae3254e.png" height="400" /> |
