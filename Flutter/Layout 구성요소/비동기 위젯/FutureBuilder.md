# FutureBuilder

FutureBuilder는 Future를 사용하는 것과 마찬가지로 데이터를 모두 다 받기 전에 먼저 데이터가 없이 그릴 수 있는 부분을 먼저 그려주기 위해 사용됩니다.

만약 FutureBuilder가 없다면 데이터가 다 받아지기 전까지 기다린 후에 화면을 그리거나 데이터의 변함을 `setState()`를 통해 바꿔줘야 합니다.

보통 FutureBuilder는 앨범에서 이미지 가져오기, 파일 가져오기, http 요청, 권한 허용 요청 등에서 사용됩니다.

<br />

## 캐싱 기능

FutureBuilder는 최초 렌더링 시 데이터가 없는 경우에만 `snapshot.data `에서 null을 반환하고 빌드가 한 번 실행되어 데이터가 있는 경우,

새롭게 빌드가 되어도 기존에 있는 데이터를 보여주고 새로운 데이터가 들어왔을 때 바뀐 데이터를 보여주는 방식을 기본적으로 내장하고 있습니다.

<br />

### FutureBuilder를 통해 로딩창 렌더링 예시

위에서 설명한 것 처럼 최초에만 데이터가 없기 때문에 Null이고 그 후엔 기존 데이터 값을 렌더링한다고 했습니다.

그런데 빌드가 실행될 때마다 로딩 창을 띄우면 사용자는 앱이 매우 느려보일 수 있습니다.

``` dart
import 'dart:math';

FutureBuilder(
  future: getNumber(),
  builder: (BuildContext context, AsyncSnapshot snapshot) {
    // ConnectionState가 waiting일 경우 실행 -> 빌드를 할 때 마다 실행
    if (snapshot.connectionState == ConnectionState.waiting) {
      return Center(
        child: CircularIndicator(),
      );
    }
    
    // 데이터를 가지고 있지 않는 경우에만 실행 -> 최초에 한번 만 실행
    if (!snapshot.hasData) {
      return Center(
        child: CircularIndicator(),
      );
    }
  }
),
  


Future<int> getNumber() async {
  await Future.delayed(Duration(seconds: 3));
  
  final random = Random();
  return random.nextInt(100);
}
```

> 아래의 코드가 더 효율적인 것을 알 수 있습니다.

<br />

## 에러가 발생한 경우

FutureBuilder에서 에러를 반환 받았을 경우에도 `Connection.state` 는 `done` 입니다.

즉 데이터를 성공적으로 받든, 에러를 받든 받기만 하면 `done` 이 되는 것을 알 수 있습니다.

<br />

## AsyncSnapshot

<br />

### 속성

- `connectionState` : 현재 연결 상태
  - none : 데이터가 없는 상태
  - done: 데이터를 받은 상태
  - waiting: 데이터를 받기 위해 기다리는 상태

<br />

- `data` : future를 통해 전달받은 데이터
- `error` : future를 통해 전달받은 에러

<br />

### 메서드

- `hasData` : snapShot이 데이터를 가지고 있는 지 여부 (bool 타입)
- `hasError` : snapShot이 에러가 발생했는 지 여부 (bool 타입)

