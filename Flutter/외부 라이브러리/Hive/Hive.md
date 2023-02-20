# Hive

NoSQL 기반 DataBase를 제공하는 라이브러리

`Hive` 는 key - value 기반의 DataBase로 가볍고, 간편하게 사용할 수 있습니다.

<br />

## Package 설정

[pub.dev](https://pub.dev/packages/hive/versions/2.0.6)와 [Hive 공식문서](https://docs.hivedb.dev/#/)를 참조하여 `pubspec.yaml` 파일에 의존성 추가를 해줍니다.

``` yaml
dependencies:
  hive: ^[version]
  hive_flutter: ^[version]
  
dev_dependencies:
  hive_generator: ^[version]
  build_runner: ^[version]
```

<br />

## Hive 초기화

Flutter에서 Hive를 사용하기 위해서는 초기화를 해줘야 합니다.

``` dart
import 'package:hive_flutter/hive_flutter.dart';

await Hive.initFlutter();
```

> Hive를 사용하려는 최상위 위젯에서 초기화 해줍니다.

<br />

## Hive Box 열기

Hive에서 Box라는 개념은 데이터를 저장하는 공간이라고 볼 수 있습니다.

SQL의 Table과 비슷한데 구조화가 되어 있지 않습니다.

``` dart
await Hive.openBox<E>('Box 이름');

final box = Hive.box<E>('Box 이름');
```

<br />

## Hive Box 사용법

- get, add, put, delete를 사용합니다.
- key를 기준으로 정렬 해줌 (기본 값 - 오름차순)

<br />

### get (read)

``` dart
// 파라미터로 넣은 key에 해당하는 value를 가져옴
box.get(key);

// 파라미터로 넣은 index에 해당하는 value를 가져옴
box.getAt(index);

// 만약 존재하지 않은 데이터를 get 할 때는 null을 리턴
// defaultValue를 사용해서 기본값을 설정할 수 있음
box.get(key, defaultValue: value);
```

> key에 해당하는 value가 없을 경우, defaultValue에 해당하는 Value를 가져옴

<br />

### add

```dart
// 파라미터로 넣은 Value가 Box의 리스트에 추가되고, key는 1씩 증가
box.add(value);
```

<br />

### put (write)

```dart
// key - value를 파라미터로 넣음
// Box에 존재하지 않은 key를 파라미터로 넣으면, 새로운 데이터를 생성
// key가 존재할 경우, 기존의 데이터를 덮어씌움

box.put(key, value);
```

<br />

### delete

```dart
// 파라미터로 넣은 key에 해당하는 값을 삭제함
box.delete(key);

// 파라미터로 넣은 Key들에 해당하는 값들을 삭제함
box.deleteAll(List);

// 파라미터로 넣은 index에 해당하는 값을 삭제함
box.deleteAt(index);
```

<br />

## Hive Adapters

Hive는 List, Map, DateTime 등의 주요 타입들은 지원해주지만 이외의 타입들은 `TypeAdapter` 를 작성하여 Hive에 등록해줘야 합니다.

``` dart
import 'package:hive_flutter/adapters.dart';

part 'student.g.dart';

@HiveType(typeId: 0)
class Student {
  @HiveField(0)
  String name;
  
  @HiveField(1)
  int age;
  
  @HiveField(2)
  String studentNumber;
  
  Student({
    required this.name,
    required this.age,
    required this.studentNumber,
  });
}
```

- `@HiveField` : 같은 typeId를 가지고 있는 HiveType 내부에서는 절대 HiveField 값들이 겹치면 안됨
- `@HiveType` : typeId를 지정해주고 다른 HiveType과 typeId가 절대로 중복되면 안됨 -> `typeId는 0 ~ 223 사이`

> hive: 2.0.4, hive_generator: 1.1.0 버전 이후 @HiveField에 default 기능 추가 됨
>
> ex - `@HiveField(1, defaultValue: 0.0)`

<br />

part 경로 추가 이후 터미널에 build_runner를 실행

``` bash
$ flutter pub run build_runner build
```

<br />

## ValueListenableBuilder

데이터가 `valueListenable` 과 동기화 된 상태로 유지되는 위젯

위젯을 빌드하는 `builder` 가 주어지면 `ValueListenableBuilder` 위젯은 자동으로 리스너를 등록하고 값이 변경되면 업데이트 된 값으로 `builder` 를 호출합니다.

``` dart
// 예시 코드
ValueListenableBuilder<Box>(
  valueListenable: Hive.box<E>(String name).listenable(),
  builder: (context, box, widget) {
    return // 위젯;
  },
)
```

> `valueListenable` : 빌드하기 위해 사용자가 의존하는 값을 리스닝
>
> `builder` : valueListenable의 값에 따라 위젯을 빌드