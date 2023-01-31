# ReorderableListView

- `ListView` 위젯과 동일한데, 사용자가 목록의 순서를 하나 하나 재배열 할 수 있습니다.
- 자식 위젯이 너무 많아지게 되면 `ReorderableListView` 위젯을 사용하기에는 적절하지 않습니다.
- ReorderableListView의 자식 위젯들은 모두 고유의 key를 가지고 있어야 합니다.

<br />

## 기본 형태

- 한 번에 모든 데이터가 렌더링 됩니다.
- 따라서 성능이 좋지 않음

``` dart
ReorderableListView(
  children: [
    // ...
  ],
  onReorder: (int oldIndex, int newIndex) {
    // ...
  },
)
```

> `oldIndex`, `newIndex` 값은 항목이 옮겨지기 전에 결정됩니다.

<br />

## builder

- **`화면에 그려지는 영역만을 랜더링한다`** (유연성을 위해 몇 개의 영역을 조금 더 렌더링)
- `itemCount`, `itemBuilder`, `onReorder` 를 통해 순서를 재 배치할 수 있는 List 구현
- `itemCount`를 통해 전체의 길이를 정의하며, `itemBuilder`에서 각각의 item을 랜더링

``` dart
ReorderableListView(
  itemCount: // int 값,
  itemBuilder: (context, index) {
    // 렌더링 될 item (위젯)
  },
  onReorder: (int oldIndex, int newIndex) {
    // 순서를 재배치 할 경우 실행될 로직
  },
)
```