# CustomScrollView

Scroll이 가능한 영역, 화면을 커스터 마이징 할 수 있는 위젯

`slivers` 리스트 안에는 Sliver 위젯들만 넣을 수 있습니다.

<br />

## 기본 형태

``` dart
CustomScrollView(
  slivers: [
    // SliverAppBar
    // SliverList
    // SliverGrid
    // 등등
  ],
)
```

<br />

## SliverAppBar

기존의 `appBar`와 거의 동일하지만, AppBar도 스크롤이 가능한 영역에 포함됩니다.

따라서 스크롤을 하여 화면을 아래로 내리게 되면 AppBar도 같이 스크롤 되어 화면에서 보이지 않게 됩니다.

``` dart
CustomScrollView(
  slivers: [
    SliverAppBar(
      title: Text('SliverAppBar'),
      floating: false, // bool (true 일 경우 -> 위로 스크롤 시 안보임, 아래로 스크롤 시 보임)
      pinned: false, // bool (true 일 경우 -> 일반 AppBar처럼 화면 상단에 고정 됨)
      snap: false,  // bool (true 일 경우 -> floating도 true 여야 적용되며, 살짝만 아래로 스크롤 해도 보임)
      stretch: false, // bool (true 일 경우 -> IOS에만 적용, 맨 위에서 한계 이상으로 스크롤 했을 때 남는 공간 차지)
      expandedHeight: 200.0, // double (AppBar의 높이 지정)
      
      // AppBar에 위젯들 추가
      flexibleSpace: FlexibleSpaceBar(
        title: Text(''),
        background: Image.asset('..'),
      ),
    ),
  ],
)
```

<br />

## SliverList

모든 데이터를 한번에 렌더링하는 (children에 위젯 전부 넣는) 방식과 화면에 보이는 것만 렌더링 하는 (builder 사용) 방식 2가지가 있습니다.

``` dart
// 모든 데이터 한번에 렌더링
CustomScrollView(
  slivers: [
    SliverList(
      delegate: SliverChildListDelegate(
        // 자식 위젯
      ),
    )
  ],
)
```

``` dart
// 화면에 보이는 부분만 렌더링
CustomScrollView(
  slivers: [
    SliverList(
      delegate: SliverChildBuilderDelegate(
        (context, index) {
          // 자식 위젯
        },
        childCount: // int 값 (리스트 구성 할 항목 개수),
      ),
    )
  ],
)
```

<br />

## SliverGrid

``` dart
CustomScrollView(
  slivers: [
    SliverGrid(
      delegate: SliverChildBuilderDelegate(
        (context, index) {
          // 자식 위젯
        },
        childCount: // int 값 (Grid 구성 할 항목 개수),
      ),
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 2,
      ),
    ),
  ],
)
```

> `gridDelegate` : GridView와 마찬가지로 `FixedCrossAxisCount`와 `MaxCrossAxisExtent`가 있습니다.
>
> `delegate` : Grid 형태를 구현할 위젯 설정 `(SliverChildBuilderDelegate, SliverChildListDelegate)`로 구성

<br />

## SliverToBoxAdapter

일반적인 Container 기반 위젯 중 하나입니다.

CustomScrollView 위젯에 여러 Container 위젯을 표시하는 대신 SliverToBoxAdaptor 위젯을 사용하는 것이 스크롤 뷰의 뷰포트를 통해 실제로 표시되는 자식 위젯만 인스턴스화 하기 때문에 더 효율적입니다.

단일 자식 위젯만 가질 수 있습니다.

``` dart
SliverToBoxAdapter(
  child: // 위젯
)
```

<br />

## SliverPadding

sliver에 Padding을 적용할 수 있는 위젯

``` dart
SliverPadding(
  padding: const EdgeInsets.all(8.0),
  sliver: SliverToBoxAdapter(
    child: // 위젯
  ),
)
```

> `sliver`에는 Sliver 관련 위젯을 넣으면 됩니다.

<br />

## SliverPersistentHeader

- `SliverAppBar` 가 아닌 별도로 헤더를 pin 하거나 float 할 수 있는 위젯
- 주로 `SliverAppBar` 아래에 탭바를 넣어서 많이 사용

``` dart
class _SliverFixedHeaderDelegate extends SliverPersistentHeaderDelegate {
  final Widget child;
  final double maxHeight;
  final double minHeight;

  _SliverFixedHeaderDelegate({required this.child, required this.maxHeight, required this.minHeight});

  @override
  Widget build(BuildContext context, double shrinkOffset, bool overlapsContent) {
    return SizedBox.expand(
      child: child,
    );
  }

  // 최대 높이
  @override
  double get maxExtent => maxHeight;

  // 최소 높이
  @override
  double get minExtent => minHeight;

  // covariant - 상속된 클래스도 사용 가능 (즉 __SliverFixedHeaderDelegate 클래스에서도 사용 가능)
  // oldDelegate - 빌드 되기 전 기존에 존재하던 Delegate
  // this - 새로 빌드된 Delegate
  // shouldRebuild - 새로 빌드를 해야 될지의 여부
  // false - 빌드 안함, true - 빌드 다시 함
  @override
  bool shouldRebuild(_SliverFixedHeaderDelegate oldDelegate) {
    return oldDelegate.minHeight != minHeight || oldDelegate.maxHeight != maxHeight || 
      oldDelegate.child != child;
  }
}


SliverPersistentHeader(
  delegate: _SliverFixedHeaderDelegate(
    child: // 위젯,
    maxHeight: // double 타입 값,
    minHeight: // double 타입 값,
  ),
)
```
