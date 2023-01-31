# ListView

선형으로 정렬된 스크롤이 가능한 위젯 목록

<br />

## 기본 형태

- `SingleChildScrollView` 와 동일하게 화면에 보이지 않은 모든 데이터도 렌더링
- 성능적으로 좋지 않음

``` dart
ListView(
  children: [
    // 위젯...
  ]
)
```

<br />

## builder

- `iteomCount`, `itemBuilder`를 통해 리스트를 구현

- `itemCount`를 통해 전체의 길이를 정의하며, itemBuilder에서 각각의 item을 랜더링

- **`화면에 그려지는 영역만을 랜더링한다`** (유연성을 위해 몇 개의 영역을 조금 더 렌더링)

``` dart
ListView.builder(
  // 화면에 렌더링 될 위젯의 전체 길이
  itemCount: // int 값,
  
  // 화면에 보여지는 리스트들을 그때 그때 마다 호출하기 위해 사용합니다. 
  // 보이는 부분만 불러오기 때문에 ListView보다 효율적으로 화면을 구성할 수 있습니다.
  itemBuilder: (context, index) {
    return //위젯
  },
)
```

<br />

## separated 

- `builder`와 동일하게 구성되나 각각 item 사이에 `구분선(separatorBuilder)`이 추가된 형태

- 리스트 사이에 구분선이 필요하거나, 리스트와 별도로 리스트 뷰에 특정 항목을 추가하여야 하는 경우 사용

``` dart
ListView.separated(
  // 화면에 렌더링 될 위젯의 전체 길이
  itemCount: // int 값,  
  itemBuilder: // builder의 itemBuilder와 동일,
  
  // itemBuilder로 생성된 item 사이에 추가되는 구분선
  separatedBuilder: (context, index) {
    return // 위젯;
  }
)
```

<br />
