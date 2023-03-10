# Freezed

데이터 클래스에서 필요한 기능들을 Code Generator를 통해 자동 생성해주는 라이브러리 입니다.

`json_serializable`, `copyWith`, `toString`, `override`, `assert` 등의 편의성 기능들을 제공합니다.

<br />

## 의존성 추가

`dependencies`에 [freezed_annotation](https://pub.dev/packages/freezed_annotation),  `dev_dependencies`에  [freezed](https://pub.dev/packages/freezed), [build_runner](https://pub.dev/packages/build_runner), [json_serialializable](https://pub.dev/packages/json_serializable)을 각각의 버전 확인 후 pubspec.yaml 파일에 등록한 뒤 pub get 해줍니다.

``` yaml
dependencies:  
  freezed_annotation: ^[version]
  
dev_dependencies:
  build_runner: ^[version]
  json_serializable: ^[version]
  freezed: ^[version]
```

<br />

## Freezed를 통한 데이터 모델 생성

1. `part` 키워드를 통해 아래와 같이 작성

``` dart
// (필수) -> User.dart 파일을 Freezed에서 생성한 코드와 연결
// 현재 파일명.freezed.dart 형식으로 선언
part 'user.freezed.dart';

// (선택) -> 직렬화를 하려면 선언
// 현재 파일명.g.dart 형식으로 선언
part 'user.g.dart';

class User {
  // ...
}
```

<br />

2. `@freezed` annotation 선언 및 `freezed_annotation` 라이브러리 Import

``` dart
import 'package:freezed_annotation/freezed_annotation.dart';

// part Code ...

@freezed
class User
```

<br />

3. `_$` 접두사가 붙은 클래스 이름과 함께 `믹스인(mixin)` 적용

``` dart
@freezed
class User with _$User {
  
}
```

<br />

4. factory 생성자 선언

``` dart
@freezed
class User with _$User {
  factory({
    required String name,
    required int age,
  }) = _User;
}
```

> factory 생성자 앞에 const를 붙히는 것은 상황에 따른 선택사항 입니다.

<br />

5. Json 직렬화를 하려면 `fromJson` 추가

``` dart
@freezed
class User with _$User {
  factory({
    required String name,
    required int age,
  }) = _User;
  
  factory User.fromJson(Map<String, dynamic> json) => _$User(json);
}
```

<br />

6. 전체 코드

``` dart
import 'package:freezed_annotation/freezed_annotation.dart';

part 'user.freezed.dart';
part 'user.g.dart';

@freezed
class User with _$User {
  factory({
    required String name,
    required int age,
  }) = _User;
  
  factory User.fromJson(Map<String, dynamic> json) => _$User(json);
}
```

<br />

<br />

## Freezed에서 기본 제공하는 기능

`toString()`, `toJson()`, `fromJson()`, `== 연산 비교`, `hasCode` 등의 기능을 기본으로 제공합니다.

<br />

### toString override, Json method 생성

``` dart
final mockUser = User(name: 'Hong', age: 24);

print(mockUser.toString());
print(mockUser.toJson());
```

> `toString()` : toString() 함수를 override해서 정보를 제공해주는 형태로 출력됨
>
> => 일반적으로 인스턴스에 toString() 실행하면 Instance of Class 형태로 출력됨
>
> `toJson()` : 해당 인스턴스를 Json 형태로 변환

<br />

### == 연산 비교 및 hasCode override

``` dart
final user1 = User(name: 'Hong', age: 24);
final user2 = User(name: 'Hong', age: 24);
final user3 = User(name: 'Hong', age: 26);

print(user1 == user2);
print(user1.hashCode == user2.hashCode);
print(user1 == user3);

/**
실행 결과
  true
  true
  false
**/
```

> `==` 연산 비교와 hashCode를 override 합니다. <- 상식적인 비교가 가능하게 됩니다.
>
> 일반적으로 인스턴스를 비교하면 메모리 위치를 비교하기 때문에 같은 클래스 인스턴스와 같은 필드 값들을 가져도 false를 가지게 됩니다.

<br />

<br />

## 추가적인 기능들

<br />

### Assert

- `@Assert` annotation을 통해 특정 Property의 값을 제한할 수 있습니다.
- `@Assert` 조건에 어긋나면 인스턴스 생성할 시 오류를 발생시킵니다.
- 여러 개를 작성하고 싶을 경우 factory 생성자 위에 이어서 작성하면 됩니다.
- `@Assert(조건, 에러 발생 시 보여줄 메시지)` 형태로 작성합니다.

``` dart
@freezed
class User with _$User {
  @Assert('name.length < 5', '이름은 5글자 미만으로 작성하세요.')
  factory User({
    required String name,
    required int age,
  }) = _User;
}

final user1 = User(name: 'Cheong', age: 3);
final user2 = User(name: 'Kim', age: 42);

// user1은 name의 길이가 5미만이 아니기 때문에 Assert 에러 발생
```

<br />

### Custom Method와 getter

- Custom Method와 getter를 설정할 경우 `internal constructor`를 필수로 추가해야 합니다.
- Setter를 설정하는 것은 불가능합니다. (freezed는 immutable(불변) 사용을 목적으로 하기 때문)
  - 한번 설정한 것이 변하지 않도록 하는 것이 사용 목적

``` dart
@freezed
class User with _$User {
  factory User({
    required String name,
    required int age,
  }) = _User;
  
  // Custom Method와 getter를 설정하기 위해 필요한 internal Constructor
  User._();
  
  get nameLength => name.length;
  
  void customMethod() {
    print('User 클래스의 Custom Method 입니다.');
  }
}

final user = User(name: 'Hong', age: '24');

print(user.nameLength);
user.customMethod();

// 실행 결과
// 4
// User 클래스의 Custom Method 입니다.
```

<br />

### copy와 deep copy

- freezed는 immutable을 목적으로 하기 때문에 Setter를 통해 값을 변경하는 것이 불가능합니다. 따라서 값을 변경하고 싶을 경우 새로운 인스턴스를 생성하거나 `copyWith` 메서드를 통해 변경할 수 있습니다.
- `Deep Copy`는 Freezed로 생성한 클래스 모델이 또 다른 Freezed로 생성한 클래스 모델을 속성으로 가지고 있을 때 해당 속성을 변경할 수 있습니다.
- 만약 일부 객체의 속성이 `null`일 경우 `?.call` 연산자를 사용할 수 있습니다.

``` dart
@freezed
class Member with _$Member {
  factory Member({
    required int id,
    required String name,
    required Team team,
  }) = _Member;
}

@freezed
class Team with _$Team {
  factory Team({
    required int id,
    required String name,
    required Company company,
  }) = _Team;
}

@freezed
class Company with _$Company {
  factory Company({
    required int id,
    required String name,
  }) = _Company;
}

final company1 = Company(id: 1, name: 'Naver');
final team1 = Team(id: 1, name: 'ATeam', company: company1);
final member1 = Member(id: 1, name: 'Hong', team: team1);

// copyWith을 통해 id만 2로 변경
final member2 = member1.copyWith(id: 2);

// Deep Copy, copyWith을 통해 Company의 name만 변경
final member3 = member1.copyWith.team.company(name: 'KaKao');

// Company의 name이 null일 경우 ?.call 연산자를 사용할 수 있음
final member4 = member1.copyWith.team.company?.call(name: 'Nexon');

print('member2: $member2');
print('member3: $member3');

/**
실행 결과
member2: Member(id: 2, name: 'Hong', team: 'ATeam', company: Company(id: 1, name: 'Naver'))
member3: Member(id: 1, name: 'Hong', team: 'ATeam', company: Company(id: 1, name: 'KaKao'))
**/
```

<br />

### Default Value (기본값 설정)

Flutter는 Factory 생성자에 기본값을 지정하는 것을 허용하지 않습니다.

따라서 속성의 기본값을 지정하려면 `@Default` annotation을 사용하면 됩니다.

``` dart
class Test with _$Test {
  factory Test([
    @Default(31) int value
  ]) = _Test;
}
```

> fromJson, toJson을 통해 직렬화 / 역직렬화를 한다면 자동으로 `@JsonKey(defaultValue: <value>)`가 추가됩니다.

<br />

## Hive와 함께 사용하여 데이터 모델링

자세한 Hive 내용은 [해당 문서](https://github.com/sejong77/Today-Learn/blob/Master/Flutter/%EC%99%B8%EB%B6%80%20%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC/Hive/Hive.md) 참조

예시 파일명 => `user_model.dart`

```dart
import 'package:freezed_annotation/freezed_annotation.dart';
import 'package:hive_flutter/hive_flutter.dart';

part 'user_model.freezed.dart';
part 'user_model.g.dart';

@freezed
abstract class UserModel with _$UserModel {
  @HiveType(typeId: 1, adapterName: 'UserAdapter')
  factory UserModel({
    @HiveField(0)
    required int id,
   
    @HiveField(1)
    required String name,
    
    @HiveField(2)
    int? age,
  }) = _UserModel;
  
  // fromJson
  factory UserModel.fromJson(Map<String, dynamic> json) => _$UserModelFromJson(json);
}
```
