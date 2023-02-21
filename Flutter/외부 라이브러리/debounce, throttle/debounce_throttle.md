# Debounce, Throttle

- 특정 함수의 실행을 제한하는 목적을 가지고 있음
- `Debounce`는 특정 기간안의 함수 실행을 모두 취소하고 마지막에만 실행함
- `Throttle`은 특정 기간안의 첫번째 함수 실행 후 추가 실행을 모두 취소함
- `Debounce, Throttle`은 이벤트가 짧은 시간 내에 복수 발생 했을 때 사용할 수 있음
- 이벤트가 반복적으로 발생하는 상황에서 `Throttle`은 중간중간에 한번씩 이벤트를 실행하고, `Debounce`는 이벤트가 끝날 때만 (혹은 이벤트가 시작할 때만) 한번 실행함으로서 성능 향상의 목적이 있음
- 시간을 입력하고, 리스너를 통해 특정 함수를 실행하도록 넣어줌

<br />

## 의존성 추가

[debounce_throttle](https://pub.dev/packages/debounce_throttle) 버전 확인 후 pubspec.yaml 파일에 등록 후 pub get

``` yaml
dependencies:
  debounce_throttle: ^[version]
```

<br />

## Debounce

- 등록을 할 때 항상 시간을 입력해야 함
- 최초에 시간을 입력하여 debounce를 등록하면 입력한 시간만큼 지연됐다가 실행됨
- 지정해놓은 시간동안 마지막 함수만 실행을 하고 그 전에 실행되는 함수는 모두 취소함
- 검색창, 갯수변경, 상품등록 등 마지막 데이터만 서버와 통신해도 되는 상황일 경우 유용함
  - ex) 검색창에 입력을 할 때 마다 서버와 통신할 필요는 없음. 따라서 Debounce를 적용하여 마지막 검색결과만 서버와 통신하면 됨

<img src="https://user-images.githubusercontent.com/68320595/218687676-f027c13d-c6ad-44e1-be9b-b86e64fe66b4.png" />

<br />

### Debounce 정의

``` dart
Debouncer(
  duration,  // 지정할 시간
  initialValue,  // 클래스 혹은 단일 값 혹은 null
  checkEquality,  // 함수 실행이 될 때 넣어주는 값이 같으면 실행할지 여부 (true - 실행 X, false - 실행 O)
)
```

<br />

### Debounce 사용 예제

```dart
// debounce 선언
final updateInputDebounce = Debouncer(
  const Duration(seconds: 1),
  initialValue: null,
  checkEquality: false,
);

// 리스너 등록
updateInputDebounce.values.listen((event) {
  searchInputData();
});

// 리스너에 등록하여 실행될 함수
Future<void> searchInputData() async{
  // code ...
}


// 적용
updateInputDebounce.setValue(null);
```

> 1. Debouncer 선언
> 2. 리스너에 등록하여 실행할 함수 정의
> 3. 리스너 등록
> 4. 적용
>
> => initialValue, setValue에 넣어주는 값은 위의 예제처럼 꼭 null일 필요는 없음

<br />

## Throttle

- debounce와 동일하게 등록할 때 시간을 입력해야 함
- debounce와 달리 throttle은 등록하면 바로 실행 됨
- 지정해놓은 시간동안 첫 번째 함수만 실행을 하고 그 뒤로 실행되는 함수는 모두 취소함 
- 지도, 페이지 스크롤링 등 적당히 실시간처럼 보이게 하면서 모든 이벤트를 서버와 통신할 필요는 없는 상황에서 유용함

<img src="https://user-images.githubusercontent.com/68320595/218687680-3911aaaf-3ea3-42fa-8949-05a307c3116a.png" />

<br />

### Throttle 정의

``` dart
Throttle(
  duration,  // 지정할 시간(기간)
  initialValue,  // 클래스 혹은 단일 값 혹은 null
  checkEquality,  // 함수 실행이 될 때 넣어주는 값이 같으면 실행할지 여부 (true - 실행 X, false - 실행 O)
)
```

<br />

### Throttle 사용 예제

``` dart
// Throttle이 적용되어 실행될 클래스
_TestInfo {
  final int count;
  final bool isMore;
  
  _TestInfo({
    this.count,
    this.isMore,
  });
}

// Throttle 선언
final testThrottle = Throttle(
  Duration(seconds: 3),
  initialValue: _TestInfo(),
  checkEquality: false,
);

// 리스너 등록
testThrottle.values.listen((state) {
  _test(state);
});

// 리스너에 등록하여 실행될 함수
_test(_TestInfo info) {
  // code..
}


// 적용
testThrottle.setValue(
  _test(count: 10, isMore: false),
);
```

> 1. Throttle 선언
> 2. 리스너에 등록하여 실행될 함수 정의
> 3. 리스너 등록
> 4. 적용
>
> => 최초에 Throttle 선언할 때 initialValue가 setValue에 넣은 값으로 바뀜
>
> => 위의 코드처럼 실행을 하면 지정된 시간인 3초 내에는 맨 처음 함수만 실행되고 그 후에 함수가 실행되면 모두 취소됨
>
> => 중복 요청을 방지할 수 있음