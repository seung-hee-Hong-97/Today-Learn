# RefreshIndicator

스크롤을 아래로 했을 때 새로고침 하는 기능을 제공하는 위젯

<br />

``` dart
RefreshIndicator(
  onRefresh: () async {
    await // 서버 요청 로직
  },
  child: // 위젯,
),
```

<br />

`RefreshIndicator` 위젯의 하위 위젯을 아래로 스크롤링 하면 새로고침 기능을 사용할 수 있습니다.
