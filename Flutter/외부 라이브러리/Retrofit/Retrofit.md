# Retrofit

Annotation 선언으로 HTTP 엔드포인트에 대한 액션들을 자동으로 클래스화 시켜주는 라이브러리

엔드포인트 별로 method를 자동으로 생성해줌

<br />

## 의존성 추가

[pub.dev](https://pub.dev/packages/retrofit)에 접속하여 버전 확인 후 pubspec.yaml 파일에 의존성 추가를 한 뒤 pub get을 해줍니다.

``` yaml
dependencies:
  dio: ^[version]
  retrofit: ^[verison]
  logger: any  #for logging purpose

dev_dependencies:
  retrofit_generator: ^[verison]
  build_runner: ^[verison]
  json_serializable: ^[verison]
```

> retrofit은 dio를 기반을 통신하며, build_runner를 통해 코드를 생성

<br />

## API 정의 및 생성

`Retrofit` 라이브러리는 `Dio`, `json_serializable` 라이브러리와 같이 쓰이는 경우가 많습니다.

``` dart
import 'package:retrofit/retrofit.dart';
import 'package:dio/dio.dart';

part 'rest_client.g.dart';

@RestApi(baseUrl: 'http://localhost:3000/test')
abstract class RestClient {
  factory RestClient(Dio dio, {String baseUrl}) = _RestClient();
  
  @GET('/{id}')
  @Header({
    'accessToken': 'true',
  })
  Future<TestDetailModel> getTestDetail({
    @Path() required String id,
  });
}
```

- `@RestApi` annotation으로 baseUrl 생성
  - baseUrl은 공통적으로 사용될 서버 Url로 설정

<br />

- `abstract` 키워드를 붙혀줘야 함

<br />

- part '파일명.g.dart' 를 추가 (build_runner를 통해 자동 생성)

<br />

- factory 생성자를 이용해 dio와 baseUrl을 파라미터로 받는 객체 생성

<br />

- 사용하려는 엔드포인트를 설정하고 메서드와 데이터 타입 설정 (GET, POST, PUT, DELETE 등의 데이터 요청 타입)

  Ex) GET 요청으로 `test/{id}` 경로로 요청, 헤더에 accessToken을 true로 설정, id 값을 파라미터로 받음

  <br />

- 실제로 반환 받는 데이터 타입과 동일한 타입으로 설정해줘야 함

<br />

- generator  실행

``` dart
$ flutter pub run build_runner build
```

<br />

## 사용

``` dart
import 'pakcage:dio/dio.dart';
import '../RestClient.dart';

class TestDetailScreen extends StatelessWidget {
  final String id;
  
  const TestDetailScreen({
    required this.id,
  });
  
  // 사용
  Future<TestDetailModel> getTestDetail() async {
    final dio = Dio();
    
    final repository = RestClient(dio);
    
    return repository.getTestDetail(id: id);
  }
  
}
```

