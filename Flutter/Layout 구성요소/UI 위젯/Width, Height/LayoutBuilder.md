# LayoutBuilder

부모 위젯 사이즈에 의존하는 위젯트리를 만듭니다.

부모 위젯이 자식 위젯의 사이즈에 제약을 둘 수 있습니다.

위젯의 사이즈에 맞게 레이아웃의 크기를 유동적으로 변경시킬 수 있습니다.

<br />

``` dart
LayoutBuilder(builder: (context, constraints) {
  final maxwidth = constraints.maxWidth;
  final maxHeight = constraints.maxHeight;
  final minWidth = constraints.minWidth;
  final minHeight = constraints.minHeight;
  
  return // 위젯;
})
```

> 보통 `maxWidth`, `maxHeight`을 많이 사용합니다.

<br />

- LayoutBuilder 위젯의 자식 위젯의 너비 혹은 높이를 지정할 때 LayoutBuilder 위젯의 너비, 높이를 참조하여 설정할 수 있습니다.
- `builder` 함수가 실행되는 경우는
  - 처음으로 위젯이 렌더링 될 때
  - 부모 위젯의 constraints가 바뀔 때
  - 부모 위젯이 해당 위젯을 업데이트 할 때

<br />

## 사용 예시

``` dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    body: LayoutBuilder(
      builder: (BuildContext context, BoxConstraints constraints) {
        return Row(
          mainAxisAlignment: MainAxisAlignment.center,
          children:[
            Container(
              width: constraints.maxWidth / 3,
              color: Colors.red,
            ),
            Container(
              width: constraints.maxWidth / 3,
              color: Colors.orange,
            ),
            Container(
              width: constraints.maxWidth / 3,
              color: Colors.yellow,
            ),
          ],
        );
      }
    ),
  );
}
```

> Container 위젯의 너비를 부모 위젯인 `LayoutBuilder` 위젯의 너비 / 3 으로 설정했기 때문에 정확하게 3등분으로 화면에 보여진 것을 확인할 수 있습니다.
>
> 이처럼 `constraints`을 통해 자식 위젯의 너비 혹은 높이를 지정할 수 있습니다.

<br />

<img src="https://user-images.githubusercontent.com/68320595/216262836-b29e1710-35f9-452e-9bda-658fc3abae6d.png" height="450" />