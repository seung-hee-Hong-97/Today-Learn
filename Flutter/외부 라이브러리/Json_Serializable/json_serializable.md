# json_serializable

JSON 직렬화 보일러플레이트를 만들어 주는 자동 소스코드 생성기 입니다.

annotation을 사용해 일반 클래스를 직렬화 합니다.

<br />

## 의존성 추가

[json_serializable 공식 문서](https://github.com/google/json_serializable.dart/tree/master/example)를 확인하여 pubspec.yaml 파일에 의존성 추가를 해준 뒤 pub get을 해줍니다.

``` yaml
dependencies:
  json_annotation: ^[version]
  
dev_dependencies:
  build_runner: ^[version]
  json_serializable: ^[version]
```

<br />

## @JsonSerializable

일반 클래스를 json_serializable 클래스로 변환하는 방법입니다.

``` dart
import 'package:json_annotation/json_annotation.dart';

part 'UserModel.g.dart';

// 변환하려는 클래스 위에 annotation 선언
@JsonSerializable()
class UserModel {
  final String name;
  final int age;
  final String address;
  
  UseModel({
    required this.name,
    required this.age,
    required this.address,
  });
  
  // map에서 새로운 User 인스턴스를 생성하기 위해 필요한 factory 생성자입니다.
  // 생성된 `_$UserFromJson()` 생성자에게 map을 전달해줍니다.
  // 생성자의 이름은 클래스의 이름을 따릅니다.
  factory UserModel.fromJson(Map<String, dynamic> json) => _$UserModelFromJson(json);
}
```

<br />

## @JsonKey

네이밍 전략을 바꿀 수 있는 annotation

``` dart
class ExampleModel {
  // 네이밍 전략을 바꾸려는 변수 위에 선언
  @JsonKey(fromJson: pathToUrl)
  final String imgUrl,
  @JsonKey(name: 'member_address')
  final String memberAddress,
  
  ExampleModel({
    required this.imgUrl,
    required this.memberAddress,
  });
  
  static pathToUrl(String value) {
    return 'https://~~~/$value';
  }
}
```

> json data에 있는 `member_address` 를 `memberAddress` 로 바꿔서 정의
>
> `imgUrl`을 `http` or `https` 가 붙은 풀 경로로 바꿔서 정의

<br />

`@JsonSerializable()` 혹은 `@JsonKey()`를 추가 및 수정한 것을 반영하려면 아래와 같은 명령어를 터미널에 입력해야 합니다.

```bash
$ flutter pub run build_runner build
```