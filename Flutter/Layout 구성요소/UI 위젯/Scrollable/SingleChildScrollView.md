# SingleChildScrollView

스크롤이 가능한 위젯이며 자식을 하나만 가질 수 있습니다.

자식 위젯이 화면에 한번에 보일 정도의 크기라면 스크롤이 되지 않지만 자식 위젯의 크기가 커서 화면에 전부 보이지 않을 경우, 스크롤을 해서 자식 위젯을 전부 볼 수 있도록 하는 기능을 가지고 있습니다.

<br />

보통은 `Row` 혹은 `Column` 위젯을 자식 위젯으로 많이 사용합니다.

그 이유는 작업을 하다보면 나열해야 하는 위젯이 많아지는데 `SingleChildScrollView` 위젯은 하나의 위젯밖에 자식으로 가지지 못해서, 여러 위젯을 자식으로 가질 수 있는 Row, Column 위젯을 사용합니다.

<br />

## physics

`SingleChildScrollView` 위젯의 스크롤 방식 설정

``` dart
SingleChildScrollView(
  physics: AlwaysScrollableScrollPhysics(),  // 항상 스크롤 허용 O
  physics: NeverScrollableScrollPhysics(),  // 항상 스크롤 허용 X
  physics: BouncingScrollPhysics(),  // IOS의 기본 스크롤처럼 적용 (Android에도 적용하면 IOS처럼 스크롤링 됨)
  physics: ClampingScrollPhysics(),  // Android의 기본 스크롤처럼 적용
)
```

<br />

## clipBehavior

스크롤 하려는 콘텐츠가 잘리지 않습니다.

만일 높이 300인 Container를 스크롤 하는데 clipBehavior를 적용하지 않는다면 Container 영역에서만 스크롤링 되는 것을 확인할 수 있습니다.

`clipBehavior: Clip.none` 을 적용하면 콘텐츠가 잘리지 않고 전체 영역에서 스크롤링 되는 것을 확인할 수 있습니다.

``` dart
SingleChildScrollView(
  clipBehavior: Clip.none,
)
```

<br />

## SingleChildScrollView 성능

화면에 보이지 않는 데이터 혹은 위젯들도 렌더링하기 때문에 성능적으로 `ListView` 에 비해 좋지 않습니다.