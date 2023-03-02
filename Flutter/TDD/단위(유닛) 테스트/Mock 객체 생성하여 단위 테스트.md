# Mockito를 사용하여 의존성들에 대한 Mock 객체 생성하기

특정 상황에서는 단위(유닛) 테스트가 웹 서비스 혹은 DB로부터 데이터를 가져오는 역할을 수행하는 특정 클래스에 의존하는 경우가 있습니다.

이럴 경우에는 여러가지 이유로 테스트를 하기가 어려워집니다.

- 웹 서비스, DB를 호출하는 것은 테스트 수행을 저하 시킴

  <br />

- 성공해야 하는 테스트 케이스일지라도 웹 서비스나 DB가 예상치 못한 결과를 반환하면 실패함. 이러한 경우를 `flaky test` 라고 함

​		<br />

- 웹 서비스, DB를 사용하여 모든 가능한 성공과 실패 시나리오를 테스트 하는 것은 매우 힘듬

  <br />

  따라서 데이터가 필요하다면 웹 서비스, DB를 사용하지 않고 mock 데이터를 생성하여 상황에 따라 특정 결과 값을 반환하도록 해야 합니다.

  실제 클래스를 대체할 테스트 클래스를 만들어 Mock 객체로 활용할 수 있지만, `Mockito` 패키지를 통해서 쉽게 생성할 수 있습니다.

<br />

## Mock 객체 생성 과정

1. `mockito`, `test`, `http` 패키지 의존성 추가
2. 테스트 할 함수 생성
3. http.Client Mock 객체와 함께 테스트 파일 생성
4. 각 조건마다 테스트 작성
5. 테스트 실행

<br />

## 의존성 추가

[mockito](https://pub.dev/packages/mockito), [test](https://pub.dev/packages/test), [http](https://pub.dev/packages/http) 라이브러리의 버전을 각각 확인 후 의존성 추가

mockito의 버전이 5.0 이상이면 [build_runner](https://pub.dev/packages/build_runner)도 추가해줘야 함

``` yaml
dependencies:
  http: ^[version]
  
dev_dependencies:
  test: ^[version]
  mockito: ^[version]
```

<br />

## 테스트 할 함수 생성

예시는 [News API](https://newsapi.org/)를 이용하여 HeadLine 뉴스 데이터를 가져오는 함수

``` dart
Future<List<NewsArticle>> fetchTopHeadLines(Dio dio) async {
  final response = await dio.get(url);
  
  if (response.statusCode == 200) {
    final result = response.data;
    
    final list = result['articles'];
    return list.map((article) => NewsArticle.fromJson(article).toList());
  } else {
    throw Exception('Failed to get news List');
  }
}
```

<br />

## mock 객체와 함께 테스트 파일 생성

프로젝트 최상단에 위치해 있는 `test` 폴더 하위에 테스트 코드를 작성할 파일을 생성합니다.

`테스트이름_test.dart` 형식으로 파일명을 설정해야 합니다.

Ex) fetch_headline_test.dart

함수 위에 annotation을 선언합니다. => `@GenerateMocks(["Mock 클래스"])`

``` dart
import 'package:dio/dio.dart';
import 'package:mockito/annotations.dart';

@GenerateMocks([Dio])
void main() {
}
```

<br />

build_runner를 실행하여 Mock 객체를 생성합니다.

``` bash
$flutter pub run build_runner build
```

> 위의 명령어를 실행하면 `test` 폴더 하위에 `fetch_headline_test.mocks.dart` 파일이 생성됩니다.

<br />

## 각 조건에 대한 테스트 작성

테스트 할 함수인 `fetchTopHeadLines`는 성공하면 `List<NewsArticle>`을 반환하고 실패하면 Exception이 발생합니다.

위의 2가지 경우를 when 함수를 이용하여 조건을 설정하고 테스트 할 수 있습니다.

``` dart
@GenerateMocks([Dio])
void main() {
  group('fetchTopHeadLines', () {
    test('fetchTopHeadLines success', () async {
      final dio = MockDio();
      
      when(dio.get(url)).thenAnswer((_) async => Future.value(
        Response(
          data: {'status': 'ok', 'totalResults': '', 'articles': []},
          statusCode: 200,
          requestOptions: RequestOptions(path: ''),
        ),
      ));
      
      expect(await WebService().fetchTopHeadLines(dio), isA<List<NewsArticle>>());
    });
    
    test('throw Exception', () {
      final dio = MockDio();
      
      when(dio.get(url)).thenAnswer((_) async => Future.value(
        Response(
          data: {'status': 'error', 'code': '', 'message': ''},
          statusCode: 401,
          requestOptions: RequestOptions(path: ''),
        ),
      ));
      
      expect(await WebService().fetchTopHeadLine(dio), throwsException);
    });
  });
}
```

<br />

## 테스트 실행

명령어를 입력하여 테스트를 실행합니다.

``` bash
$ flutter test test/테스트 파일 명
```
