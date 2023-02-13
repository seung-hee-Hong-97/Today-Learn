# Code Generator

코드 생성에 의존하여 Provider를 제공하기 위한 Riverpod용 사이드 패키지

<br />

## 의존성 추가

[riverpod_generator](https://pub.dev/packages/riverpod_generator), [riverpod_annotation](https://pub.dev/packages/riverpod_annotation) 라이브러리를 pubspec.yaml 파일에 추가한 후 pub get

```yaml
dependencies:
  flutter_riverpod: ^[version]
  riverpod_annotation: ^[version]
  
  
dev_dependencies:
  build_runner: ^[version]
  riverpod_generator: ^[version]
```

<br />

## Code Generator 사용 이유

1) 어떤 Provider를 사용할지에 대한 고민없이 알아서 정해줌

   => Provider, FutureProvider

   => StateNotifierProvider는 명시적으로 Code Generator를 할 수 있음

2. 파라미터 -> Provider의 `.family` modifier를 일반 함수처럼 사용할 수 있음

<br />

## 기본 설정

Code Generator를 통해 Provider를 자동 생성하려는 파일에 `path '파일명.g.dart'` 를 명시하고 `flutter pub run build_runner build` 를 해줘야 함

``` dart
path '파일명.g.dart';
```

```bash
$ flutter pub run build_runner build
```

<br />

## @riverpod

- `@riverpod` annotation을 사용하려는 함수 위에 선언
- 함수 파라미터로는 함수명 맨 앞 글자를 대문자로 바꿔준 뒤 `함수명Ref ref` 를 넣어줌

``` dart
// Code Generator를 사용하지 않고 String 타입 값 반환하는 케이스
final testProvider = Provider<String>((ref) => 'Hello Code Generator');

// @riverpod을 통한 Code Generator 적용
@riverpod
String gState(GStateRef ref) {
  return 'Hello Code Generator';
}
```

> .g.dart 파일을 보면 `gStateProvider`가 자동생성 되어 있음

<br />

## @RiverPod, keepAlive

`@riverpod` annotation을 통해 Provider를 자동생성하여 `.g.dart` 파일을 살펴보면 기본적으로 `AutoDispose`가 적용되어 있습니다.

즉 `FutureProvider`로 생성된 Provider는 데이터 캐싱이 되지 않고 자동 제거 되는 것을 의미합니다.

``` dart
// gStateFuture 함수로 생성된 Provider는 데이터 캐싱이 유효하지 않음
// AutoDispose가 적용되어 있기 때문
@riverpod
Future<int> gStateFuture(GStateFutureRef ref) async {
  await Future.delayed(Duration(seconds: 2));
  
  return 10;
}

// @Riverpod annotation안에 keepAlive를 true로 설정하면 AutoDispose가 적용되어 있지 않은 Provider가 생성됨
// 즉 gStateFuture로 생성된 FutureProvider는 데이터 캐싱이 유효함
@Riverpod(
  keepAlive: true
)
Future<int> gStateFuture2(GStateFuture2Ref ref) async {
  await Future.delayed(Duration(seconds: 2));
  
  return 20;
}
```

> .g.dart 파일을 보면 `gStateFutureProvider` 가 자동생성되어 있고,
>
> `keepAlive: true`를 통해 생성된 Provider는 AutoDisposeFutureProvider가 아닌 FutureProvider

<br />

## 일반 함수처럼 사용

원래 Provider에 파라미터를 받으려면 `.family` modifier를 통해 값을 하나만 받아올 수 있습니다.

그런데 여러 개의 값을 받고 싶으면 클래스를 생성하고, 생성자를 정의하여 파라미터를 받는 작업을 해야 하는데 Code Generator를 통해 일반 함수처럼 함수 자체에서 파라미터를 받을 수 있습니다.

``` dart
// Code Generator를 사용하지 않고 파라미터를 여러 개 받는 케이스
class Parameter {
  final int num1;
  final int num2;
  
  Parameter({
    required this.num1,
    required this.num2,
  });
}

final testProvider = Provider.family<int, Parameter>((ref, Parameter) => Parameter.num1 + Parameter.num2);


// Code Generator를 사용하여 일반 함수처럼 파라미터 여러 개 받는 케이스
@riverpod
int gStatePlus(GStatePlusRef ref, {
  required int num1,
  required int num2,
}) {
  return num1 + num2;
}
```

> .g.dart 파일을 보면 `gStatePlusProvider`가 자동생성 되어있음

<br />

## StateNotifierProvider를 Code Generator로 생성

StateNotifierProvider 같은 경우는 Provider, FutureProvider와 생성 방식이 조금 다릅니다.

클래스를 생성하고 해당 클래스명 앞에 `_$`를 붙힌 클래스를 상속 받는 형태로 정의해야 합니다.

``` dart
@riverpod
class GStateNotifier extends _$GStateNotifier {
  // 빌드 되었을 시 초기값 설정
  @override
  int build() {
    return 0;
  }
  
  increment() => state++;
  decrement() => state--;
}
```

> .g.dart 파일을 보면 GStateNotifierProvider가 자동생성 되어있음
