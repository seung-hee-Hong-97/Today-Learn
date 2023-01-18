# Widget 이론 및 라이프사이클

화면상의 레이아웃이나 아이콘뿐만 아니라 스타일, 애니메이션까지도 Flutter에서는 위젯이라고 칭합니다.

대표적인 위젯들은 다음과 같습니다.

- `레이아웃` : Scaffold, Stack, Row, Column
- `구조` : Button, Toast, MenuDrawer
- `스타일` : TextStyle, Color
- `애니메이션` : FadeInPhoto, Transform
- `위치, 정렬` : Center, Padding

이러한 위젯들을 조합하여 앱을 구현하는 방식입니다.

위젯들은 크게 `StatelessWidget`, `StatefulWidget` 으로 구분할 수 있습니다.

<br />

## Widget의 특징

- Widget은 모두 `불변`의 법칙을 갖고 있습니다.
  - 한번 선언된 Widget은 절대 변하면 안된다는 것을 의미합니다.
- 만약 Widget의 변경이 필요하다면 기존 Widget을 삭제하고 완전 새로운 Widget을 선언해야합니다.

<br />

## StatelessWidget 라이프 사이클

상태(State)가 없기 때문에 상태 관리를 할 필요가 없습니다.

- Constructor로 생성이 되고 생성이 되자마자 build 함수가 실행됩니다.
- 위젯의 변경이 필요하면 새로운 위젯을 만들어버립니다.
- 하나의 StatelessWidget은 라이프 사이클 동안 단 한번만 build 함수를 실행합니다.

<br />

### 기본 형태

Android Studio에서 `stless`라고 입력한 뒤 Enter 키를 누르면 아래의 코드가 자동 생성 됩니다.

class 명은 본인이 원하는 것으로 입력하시면 됩니다.

``` dart
class Example extends StatelessWidget {
  const Example({Key? key}) : super(key: key);  // 1. Constructor 생성

  // 2. build 함수 실행
  @override
  Widget build(BuildContext context) {
    return Container(...);
  }
}
```

StatelessWidget을 상속받고, build라는 함수를 override 하는 걸 확인할 수 있습니다.

<br />

> ### constructor (생성자)
>
> - 기본 생성자로서, class명과 동일한 이름을 가집니다. 
> - 뒤에 붙은 `: super(key: key)`는 부모인 StatelessWidget의 기본 생성자를 호출하는 것입니다.
> - 데이터를 전달할 파라미터가 없다면 기본생성자는 `생략`할 수 있습니다.

> ### build
>
> Widget을 리턴하는 함수로서 모든 위젯 클래스에 포함되어야 하는 필수 메서드입니다. build가 리턴하는 위젯들로 View가 그려집니다.

<br />

## StatefulWidget의 라이프 사이클

<img src="https://user-images.githubusercontent.com/68320595/213097287-ff1a4bb4-df0c-42c5-82fb-a167bcb8325f.png" height="400" />

<br />

## 파라미터가 바뀌었을 때 StatefulWidget의 라이프 사이클

StatefulWidget이 생성된 후에 파라미터가 변경되었을 경우의 라이프 사이클입니다.

이미 State가 있기 때문에 `createState`로 가지 않고 `didUpdateWidget` 을 실행하는 것을 알 수 있습니다.

<img src="https://user-images.githubusercontent.com/68320595/213098133-33f55bc4-469d-49e6-ba4b-3b11c0608952.png" height="400" />

<br />

## setState를 실행 했을 때 StatefulWidget의 라이프 사이클

파라미터가 변경된 경우는 외부에서 인스턴스를 생성할 때 넣어주는 것인데, State는 내부에서 `setState` 함수를 실행해서 자체적으로 build를 재 실행 합니다.

<img src="https://user-images.githubusercontent.com/68320595/213098852-d1e6f1de-2c40-46fb-aa90-a855cd48bbd8.png" height="400" />