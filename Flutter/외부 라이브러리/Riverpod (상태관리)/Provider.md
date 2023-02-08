# RiverPod

상태관리를 위한 라이브러리

<br />

## 의존성 추가

[pub.dev](https://pub.dev/packages/flutter_riverpod)에 들어가서 flutter_riverpod을 복사하여 pubspec.yaml 파일에 의존성 추가한 뒤 pub get을 해줍니다.

``` yaml
dependencies:
  flutter_riverpod: ^[version]
```

<br />

## Provider의 종류

- Provider
- StateProvider
- StateNotifierProvider
- FutureProvider
- StreamProvider
- ChangeNotifierProvider (Provider 마이그레이션 용도)

> 각각 다른 타입을 반환해주고 사용 목적이 다름
>
> 모든 Provider는 전역적으로 선언됨

<br />

## Provider

- 가장 기본 형태의 Provider
- 읽기만 가능하며 값을 변경할 수 없음
- 아무 타입이나 반환 가능
- Service, 계산한 값등을 반환할 때 주로 사용
- 반환 값을 캐싱할 때 유용하게 사용 => 빌드 횟수 최소화 가능
- 여러 Provider의 값들을 묶어서 한번에 반환 값을 만들어 낼 수 있음

``` dart
final valueProvider = Provider<int>((ref) => 0);
```

<br />

## StateProvider

- 내부 상태는 state로 접근이 가능하고, 사용하고자 하는 위젯에서 state 값을 직접 변경 가능
- 단순한 형태의 데이터만 관리 (int, double, String 등)
- Map, List 등 복잡한 형태의 데이터는 다루지 않음
- 복잡한 로직이 필요한 경우 사용 안함 => number++ 정도의 간단한 로직으로만 한정

``` dart
final counterStateProvider = StateProvider<int>((ref) => 0);
```

<br />

 ## StateNotifierProvider

- 상태 뿐만 아니라 로직을 함께 저장할 때 사용됨 => Ex) 다른 Provider와 결합하거나, 내부에서 사용할 로직을 정의할 때 
- 복잡한 형태의 데이터 관리 가능 (클래스의 메소드를 이용한 상태관리)
- StateNotifier를 상속한 클래스를 반환

``` dart
final counterStateNotifierProvider = StateNotifierProvider<Counter, int>((ref) => Counter());

class Counter extends StateNotifier<int> {
  Counter() : super(0);
  
  void increment() => state++;
  void decrement() => state--;
}
```

> 1. `StateNotifier` 클래스를 무조건 상속 받아야 함
> 2. `StateNotifier` 클래스에 상태 관리를 할 타입을 Generic 타입으로 설정해줘야 함
> 3. 생성자를 선언해줘야 하고, `super()` 내부에 `처음에 어떤 값으로 초기화 할지` 넣어줘야 함
> 4. `StateNotifierProvider`의 Generic 타입에는 `<StateNotifier를 상속받은 클래스, StateNotifier의 Generic 타입>` 형태로 선언
> 5. `StateNotifierProvider` 의 반환값은 StateNotifier를 상속받은 클래스를 반환해주면 됨

<br />

## FutureProvider

- Future 타입만 반환 가능
- API 요청의 결과를 반환할 때 주로 사용
- FutureBuilder와 마찬가지로 데이터 캐싱이 됨
- 복잡한 로직 또는 사용자의 특정 행동 뒤에 Future를 재실행하는 기능이 없음 => 필요할 경우 StateNotifierProvider 사용
- 사실상 별로 유용하지 않은 Provider

``` dart
// future_provider.dart

final multiplesFutureProvider = FutureProvider<List<int>>((ref) async {
  await Future.delayed(Duration(seconds: 2));
  
  return [1, 2, 3, 4, 5];
});
```

``` dart
// future_provider_screen.dart

class FutureProviderScreen extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // multiplesFutureProvider의 반환값 업데이트 일 시 build 함수 재 실행
    final  state = ref.watch(multiplesFutureProvider); 
    
    return Column(
      children: [
        state.when(
          data: () {},  // 데이터 반환 받았을 경우 실행
          error: () {},  // 에러 발생할 경우 실행
          loading: () {},  // 로딩 중일 경우 실행
        ),
      ],
    );
  }
}
```

<br />

## StreamProvider

- Stream 타입만 반환 가능
- API 요청의 결과를 Stream으로 반환할 때 주로 사용 => Socket과 같은 특정 상황에서만 유용

``` dart
final multipleStreamProvider = StreamProvider<List<int>>(
  (ref) async* {
    for (int i = 0; i < 10; i++) {
      await Future.delayed(Duration(seconds: 2));
      yield List.generate(3, (index) => index * i);
    }
  },
);
```

<br />

## 전체적인 비교

| Provider 종류         | 반환값                         | 사용 예제                                      |
| --------------------- | ------------------------------ | ---------------------------------------------- |
| Provider              | 아무 타입                      | 데이터 캐싱                                    |
| StateProvider         | 아무 타입                      | 간단한 상태 관리                               |
| StateNotifierProvider | StateNotifier를 상속한 값 반환 | 복잡한 상태 관리 (실질적으로 가장 많이 사용됨) |
| FutureProvider        | Future 타입                    | API 요청의 Future 결과값                       |
| StreamProvider        | Stream 타입                    | API 요청의 Stream 결과값                       |

<br />

 ## ref.watch

- 반환값의 업데이트가 있을 때 지속적으로 build 함수를 다시 실행해줌
- 필수적으로 UI 관련 코드에만 사용해야 함
- 비동기적으로 호출하거나, `onTab`, `initState` 등의 생명주기에서는 사용하면 안됨

``` dart
class StateProviderScreen extends ConsumerWidget {
  
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final provider = ref.watch(numberProvider);
  }
}
```

<br />

## ref.read

- 실행되는 순간 단 한번만 provider 값을 가져옴
- onPressed 콜백함수처럼 특정 액션 뒤에 실행되는 함수 내부에서 사용함

```dart
ElevatedButton(
  onPressed: () {
    ref.read(numberProvider.notifier).update((state) => state + 1);  // 방법 1
  },
  child: Text('Up'),
),
ElevatedButton(
  onPressed: () {
    ref.read(numberProvider.notifier).state = ref.read(numberProvider.notifier).state - 1;  // 방법 2
  },
  child: Text('Down'),
),
```

<br />

## ref.listen

- Provider의 값이 변경되면 값을 읽는 것이 아닌 정의한 함수를 실행함
- ref.watch와 마찬가지로 build, Provider 내부에서 사용되어야 함

```dart
@override
Widget build(BuildContext context) {
  ref.listen<반환 타입>(provider, (previous, next) {
    // .. code
  });
  
  return // 위젯;
}
```

<br />

## ref.invalidate

- invalidate에 파라미터로 넣은 Provider는 유효하지 않게 됨
- 즉 초기화 된 상태로 돌아감

``` dart
ElevatedButton(
  onPressed: () {
    ref.invalidate(Provider를 넣어요);
  },
  child: Text('InValidate'),
),
```

<br />

## select

Widget, Provider의 재빌드 수를 감소시켜 최적화 시킬 때 사용

특정 속성만 구독할 때 사용

<br />

## Provider를 사용하기 위한 기본설정

1. `main.dart` 파일에서 runApp 내부에 `ProviderScope` 선언

``` dart
void main() {
  runApp(
    ProviderScope(
      child: MaterialApp(
        home: //
      ),
    ),
  );
}
```

<br />

2. **`StatelessWidget`** 인 경우 Provider를 사용하려는 클래스는 ConsumerWidget을 상속 및 build 함수 파라미터에 `WidgetRef ref` 추가

``` dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

class TestStateNotifierProviderScreen extends ConsumerWidget {
  
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // ...
  }
}
```

<br />

3. **`StatefulWidget`** 인 경우 Provider를 사용하려는 클래스는 ConsumerStatefulWidget을 상속 및 State 부분을 ConsumerState로 변경

``` dart
                                 // 여기 변경
class TestProviderScreen extends ConsumerStatefulWidget {
  const TestProviderScreen({Key? key}) : super(key: key);

  @override
  ConsumerState<TestProviderScreen> createState() => _TestProviderScreenState();
  // 여기 변경
}

                                       // 여기 변경
class _TestProviderScreenState extends ConsumerState<TestProviderScreen> {
  @override
  Widget build(BuildContext context) {
    return const Placeholder();
  }
}
```

<br />

## Provider 결합

여러 Provider를 결합하여 사용할 수 있는데, `ref`객체를 사용하여 콜백을 전달하거나 `watch` 메소드를 사용할 수 있습니다.

``` dart
final cityProvider = Provider((ref) => 'Seoul');

final weatherProvider = FutureProvider((ref) async {
  final city = ref.watch(cityProvider);
  
  return fetchWeather(city: city);
});
```

<br />

## ProviderObserver

Provider의 변경사항을 listen 합니다. ProviderObserver를 상속받는 Class를 만들어서 ProviderScope 생성 시 observers에 넘겨주면 됩니다.

observers에는 ProviderObserver를 여러 개 사용할 수 있습니다.

Class 선언 시 3개의 함수를 override 할 수 있습니다.

``` dart
class Logger extends ProviderObserver {
  @override
  void didUpdateProvider(
    ProviderBase provider, 
    Object? previousValue, 
    Object? newValue, 
    ProviderContainer container) {
    
  }
  
  @override
  void didAddProvider(ProviderBase provider, Object? value, ProviderContainer container) {
    
  }
  
  @override
  void didDisposeProvider(ProviderBase provider, ProviderContainer container) {
    
  }
}
```

> `didUpdateProvider` : provider가 변경될 때 마다 호출됩니다. provider의 이전 value와 새로운 value를 파라미터로 받습니다.
>
> `didAddProvider` : provider가 초기화 될 때 마다 호출됩니다. provider의 value를 파라미터로 받습니다.
>
> `didDisposeProvider` : provider가 dispose(해제) 될 때 마다 호출됩니다. 











