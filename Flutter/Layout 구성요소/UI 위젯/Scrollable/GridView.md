# GridView

자식 위젯들의 사이즈와 포지션을 컨트롤하고, `SliverGridDelegateWithFixedCrossAxisCount` 와 `SliverGridDelegateWithMaxCrossAxisExtent` 2가지 종류가 있습니다.

<br />

## 기본 형태

- 화면에 보이지 않는 데이터도 렌더링 함
- 따라서 성능이 떨어짐

``` dart
GridView.count(
  crossAxisCount: 2,
  crossAxisSpacing: 12.0,
  mainAxisSpacing: 12.0,
  childAspectRatio: 2.0,
)
```

<br />

## builder

ListView의 builder와 비슷한 개념으로, **`화면에 그려지는 영역만을 랜더링한다`** (유연성을 위해 몇 개의 영역을 조금 더 렌더링)

GridView의 builder에는 `gridDelegate`가 추가됩니다.

- `itemCount`, `itemBuilder`, `gridDelegate` 를 통해 Grid 구현
- `itemCount`를 통해 전체의 길이를 정의하며, itemBuilder에서 각각의 item을 랜더링
- `gridDelegate` : 어떤 방식으로 Grid 배치를 할 것인지 설정

<br />

## gridDelegate 2가지 종류

### SliverGridDelegateWithFixedCrossAxisCount

몇 개를 배치할지 결정합니다.

- `crossAxisCount` : crossAxis 방향으로 몇 개의 Grid를 배치할 것인지 결정 (int 타입)
- `crossAxisSpacing` : Grid 사이의 좌우 간격 (double 타입)
- `mainAxisSpacing` : Grid 사이의 수직(상하) 간격 (double 타입)
- `childAspectRatio` : 자식 위젯의 가로 세로 비율 (double 타입)

``` dart
GridView.builder(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 2,
    crossAxisSpacing: 12.0,
    mainAxisSpacing: 12.0,
    childAspectRatio: 1.0,
  ),
  itemBuilder: (context, index) {
    return // 위젯;
  },
  itemCount: // int 값,
)
```

<br />

### SliverGridDelegateWithMaxCrossAxisExtent

반응형으로 구현하고 싶을 때 사용합니다.

`maxCrossAxisExtent` 와 `기기의 width`를 이용하여 crossAxis으로 grid가 몇 개가 들어갈지 지정합니다.

- `maxCrossAxisExtent` : 자식 위젯에게 부여할 최대 너비 지정 (double 타입)
- `crossAxisSpacing` : Grid 사이의 좌우 간격 (double 타입)
- `mainAxisSpacing` : Grid 사이의 수직(상하) 간격 (double 타입)
- `childAspectRatio` : 자식 위젯의 가로 세로 비율 (double 타입)

``` dart
GridView.builder(
  gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
    maxCrossAxisExtent: 2.0,
    crossAxisSpacing: 12.0,
    mainAxisSpacing: 12.0,
    childAspectRatio: 1.0,
  ),
  itemBuilder: (context, index) {
    return // 위젯;
  },
  itemCount: // int 값,
)
```
