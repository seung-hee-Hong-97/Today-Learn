# TabBar

가로 형태로 여러 디스플레이들을 한 화면에서 제어할 수 있는 위젯

보통 `TabBar`는 단독으로 사용하지 않고 대부분 선택된 화면을 보여주는 `TabBarView`와 이 둘을 제어할 수 있는 `TabController` 클래스와 함께 사용됩니다.

TabBar의 종류는 AppBar의 바로 밑에 나타나는 형태와 화면 하단에 위치하는 `bottomNavigationBar` 위젯이 있습니다.

현재 문서에서 다뤄 볼 내용은 `bottomNavigationBar` 위젯입니다.

<br />

`TabController` 는 BottomNavigationBar와 TabBarView의 동기화를 시켜주는 역할을 합니다.

<br />

## BottomNavigationBar

`BottomNavigationBar` 위젯의 `items` 속성의 자식 위젯으로 `BottomNavigationBarItem` 위젯을 여러 개 넣어도 되고, List builder를 써서 구현해도 됩니다.

``` dart
bottomNavigationBar: BottomNavigationBar(
  selectedItemColor: Color 값, // 선택된 Tab의 전체 색상
  unselectedItemColor: Color 값,  // 선택되지 않은 Tab의 전체 색상
  selectedFontSize: double 값,  // 선택된 Tab의 글자 크기
  unselectedFontSize: double 값,  // 선택되지 않은 Tab의 글자 크기
  type: BottomNavigationBarType.fixed or shifting,  // 선택된 Tab이 확대되고 영역을 많이 차지하는 이펙트를 줄 것인지
  onTap: (int index) { },  // 탭 클릭 시 발생하는 함수
  currentIndex: int 값,  // 현재 index
  items: 리스트 형태의 위젯,  // BottomNavigationBar에 들어갈 Tab 항목 위젯들
)
  
  
// items에는 보통 아래와 같이 설정합니다.
[
  BottomNavigationBarItem(
    icon: Icon(Icons.값),
    label: 'ddd',
  ),
]
```

<br />

## TabController

전체 탭의 개수인 `length`와 현재 controller를 관리하는 State 혹은 StatefulWidget인 `vsync` 가 필수 파라미터입니다.

``` dart
class _TestTabState extends StatefulWidget with SingleTickerProviderStateMixin {
  late TabController controller;
  
  @override
  void initState () {
    super.initState();
   
    controller = TabController(length: int 값, vsync: this);
  }
}
```

> 보통 controller를 관리하는 곳은 자기 자신인 경우가 많기에 this를 사용합니다.
>
> 하지만 vsync에 this를 넣으려면 State에 `with SingleTickerProviderStateMixin` 을 추가해줘야 합니다.

<br />

## TabBarView

Tab의 개수인 `length`의 숫자만큼 자식 위젯을 가져야 합니다.

드래그 옵션, 화면 잘림 여부를 결정하는 옵셩 등 다양한 속성을 가지고 있습니다.

``` dart
TabBarView(
  physics: // 드래그 옵션
  controller: // BottomNavigationBar와 TarBarView의 동기화를 시켜줌
  children: [
    // Tab의 개수만큼 자식 위젯 생성
  ],
)
```





