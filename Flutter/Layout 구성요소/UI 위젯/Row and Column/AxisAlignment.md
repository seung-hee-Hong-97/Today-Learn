# AxisAlignment

`axis`는 중심축 이라는 뜻입니다.

AxisAlignment는 주축과 횡축을 정렬하는 2가지가 있고 각각 주축으로는 `MainAxisAlignment`, 횡축으로는 `CrossAxisAlignment` 가 있습니다.

주축은 `Row`, `Column` 위젯에서 사용되는 속성입니다.

<br />

## MainAxisAlignment

`MainAxisAlignment` 는 주축을 정렬하는 Row / Column 위젯의 속성으로 총 6가지의 값들이 있습니다.

기본 값은 start 입니다.

- `start` - Column일 경우 상단, Row일 경우 왼쪽
- `center` - 가운데
- `end`- Column일 경우 하단, Row일 경우 오른쪽
- `spaceBetween` - 끝과 끝에 위젯이 배치가 되고 위젯과 위젯의 사이가 동일하게 배치
- `spaceEvenly` - 위젯을 같은 간격으로 배치하지만 끝과 끝에도 위젯이 아닌 빈 간격으로 배치
- `spaceAround` - spaceEvenly와 거의 동일하고, 차이는 끝과 끝의 간격이 1/2 정도로 배치

``` dart
// 나머지 설정 코드는 생략하고 mainAxisAlignment 코드만 작성

Column(
  mainAxisAlignment: MainAxisAlignment.정렬 값
)
  
// or
  
Row(
  mainAxisAlignment: MainAxisAlignment.정렬 값
)
```

> `정렬 값` : 위에 작성된 6가지 값들 중 하나를 작성하면 됩니다.

<br />

### Column

**start**<img src="https://user-images.githubusercontent.com/68320595/212813113-be6f6fcd-bd6c-4de3-a490-a379860dba59.png" width="100" />    **end**  <img src="https://user-images.githubusercontent.com/68320595/212813128-84a81e52-59fc-4c2c-bf4b-2648444a07d1.png" width="100" />    **center**  <img src="https://user-images.githubusercontent.com/68320595/212813143-19be7ffd-6a33-4048-9131-c95d1dba833f.png" width="100" />      **spaceBetween**<img src="https://user-images.githubusercontent.com/68320595/212813154-07771ef2-1e2f-48e7-a294-5337394b9339.png" width="100" />          

**spaceEvenly**<img src="https://user-images.githubusercontent.com/68320595/212813161-d0fc27a6-6a52-4b49-a9bb-5438039b0b9e.png" width="100" />        **spaceAround**<img src="https://user-images.githubusercontent.com/68320595/212813170-8c994a73-bc19-44f4-81b0-525fc643c110.png" width="100" />

<br />

### Row

**start** <img src="https://user-images.githubusercontent.com/68320595/212814376-50d6cf9f-ae19-42ea-aed6-cb1ae8967d47.png" width="200" />  **end** <img src="https://user-images.githubusercontent.com/68320595/212814388-34be720c-c10e-421b-8f54-22325ebbe81a.png" width="200" />  **center** <img src="https://user-images.githubusercontent.com/68320595/212814393-460596cf-8474-48ba-9166-3c897670e73a.png" width="200" />

**spaceBetween** <img src="https://user-images.githubusercontent.com/68320595/212814406-ee040d3d-0580-426c-97fa-e6a4e483c837.png" width="200" />	**spaceEvenly** <img src="https://user-images.githubusercontent.com/68320595/212814412-dccbbcb2-e8b3-40e8-9c2f-f0bd5bca6285.png" width="200"/>  

**spaceAround** <img src="https://user-images.githubusercontent.com/68320595/212814421-68d02b69-561b-4cf1-8f3a-56f352c56429.png" width="200"/>

<br />

## CrossAxisAlignment

`CrossAxisAlignment` 는 주축의 반대축을 정렬하는 Row / Column 위젯의 속성으로 총 5가지의 값들이 있습니다.

즉 `Row` 위젯일 경우 세로축을 `Column` 위젯일 경우 가로축이 CrossAxisAlignment의 기준축이 됩니다.

기본 값은 Center 입니다.

- `baseline` - 글자를 배치할 때 사용
- `start` - Column일 경우 왼쪽 정렬, Row일 경우 상단
- `end` - Column일 경우 오른쪽 정렬, Row일 경우 하단
- `center` - Column일 경우 가로축으로 가운데, Row일 경우 세로축으로 가운데
- `stretch` - Column일 경우 가로축, Row일 경우 세로축으로 최대한 너비 혹은 높이를 늘림

``` dart
// 나머지 설정 코드는 생략하고 crossAxisAlignment 코드만 작성

Column(
  crossAxisAlignment: CrossAxisAlignment.정렬 값
)
  
// Or

Row(
  crossAxisAlignment: CrossAxisAlignment.정렬 값
)
```

> `정렬 값` : 5가지 값들 중 하나를 작성하면 됩니다.

<br />

### Column

**start** <img src="https://user-images.githubusercontent.com/68320595/212815364-f8ded97c-ab7e-4765-a0f0-25a68d84c31c.png" width="150" />   **end** <img src="https://user-images.githubusercontent.com/68320595/212815374-e74b5fbf-2dea-466e-9529-c418758baad1.png" width="150"/>    **center** <img src="https://user-images.githubusercontent.com/68320595/212815384-ad7bd6cb-955e-45b8-b0aa-89f1fefbe5d1.png" width="150"/>    **stretch** <img src="https://user-images.githubusercontent.com/68320595/212815393-e29e6d29-3dfe-4229-bd2e-7aab97633d32.png" width="150"/>

<br />

### Row

**start** <img src="https://user-images.githubusercontent.com/68320595/212815873-da286c09-6eb7-468e-bb10-b84f07b8f8af.png" height="200" width="100" />	**end** <img src="https://user-images.githubusercontent.com/68320595/212815879-b2ab3aa9-599a-4b6d-9a8e-1cab62883956.png" height="200" width="100" />  **center** <img src="https://user-images.githubusercontent.com/68320595/212815891-b2c77536-db5c-4a61-9c1f-307cf3374597.png" height="200" width="100" />     **stretch** <img src="https://user-images.githubusercontent.com/68320595/212815901-5b68e772-0c8a-4f68-9a55-6fc78d9ede34.png" height="200" width="100" />     

<br />

## MainAxisSize

`MainAxisSize`는 주축의 크기를 조절하는 Column / Row 위젯의 속성으로 2가지의 값이 있습니다.

- `max` : 주축을 기준으로 최대 크기
- `min`: 주축을 기준으로 최소 크기

``` dart
Column(
  mainAxisSize: MainAxisSize.크기 값
)
  
// or

Row(
  mainAxisSize: MainAxisSize.크기 값
)
```

