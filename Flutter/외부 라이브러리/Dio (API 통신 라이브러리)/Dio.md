# Dio

서버와 API 통신을 하기 위해 필요한 라이브러리

<br />

## Package 설정

[pub.dev](https://pub.dev/packages/dio)에 접속하여 Dio 라이브러리를 복사하여 pubspec.yaml 파일에 의존성 추가를 해줍니다.

``` yaml
dependencies:
  dio: verison
```

<br />

## 주요 기능

## 1. Request & Response

```dart
// 1. 일반적인 Get
final response = await Dio().get(URL);

// 2. Get에 queryParameters 포함
final response = await Dio().get(URL, queryParameters: {'id': 1, 'name': 'Hong'});

// 3. request 사용
final response = await Dio().request(
  URL,
  data: {'id': 1, 'name': 'Hong'},
  options: Options(method: 'GET'),
);

// 4. POST 사용
final response = await Dio().post(URL, data: {'id': 1, 'name': 'Hong'});
```

> - request와 response는 위의 코드와 같이 요청 메소드와 Url을 같이 작성해주면 됩니다.
> - 각 메소드 별로 함께 넘길 수 있는 여러 Parameter들이 있습니다.
> - 요청하는 방법에는 여러가지가 있는데, 위의 코드처럼 `queryParameters`를 사용해도 되고, `body` 를 사용해도 됩니다.

<br />

## 2. Options

```dart
final dio = Dio();

dio.options.baseUrl = '기본 URL 설정';
dio.options.connectTimeout = 4000; // 4초
dio.options.receiveTimeout = 3000; // 3초

final options = BaseOptions(
  dio.options.baseUrl = '기본 URL 설정';
  dio.options.connectTimeout = 4000; // 4초
  dio.options.receiveTimeout = 3000; // 3초
);

Dio dio = Dio(options);
```

dio 객체를 생성하면서 공통으로 사용하고 싶은 것들은 `BaseOptinos`를 통해 지정할 수 있습니다.

주로 사용하는 옵션들은 다음과 같습니다.

- `baseUrl` : 요청할 기본 주소를 설정할 수 있음
- `connectTimeout` : 서버로부터 응답받는 시간을 설정
- `receiveTimeout` : 파일 다운로드와 같이 연결 지속시간을 설정할 수 있음
- `headers` : request의 header 데이터를 설정할 수 있음 -> ex) 인증 토큰

<br />

## 3. Interceptor

요청을 할 때 마다 `Interceptor`에 설정한 로직이 실행 되어 요청 때 마다 반복적으로 실행되어야 하는 작업을 간단하게 처리할 수 있습니다.

ex) 토큰 유효성 검사, 로그 처리 등등

``` dart
class Interceptor {
  void onRequest(
    RequestOptions options,
    RequestInterceptorHandler handler,
  ) => handler.next(options);
  
  void onResponse(
    Response response,
    ResponseInterceptorHandler handler,
  ) => handler.next(response);
  
  void onError(
    DioError err,
    ErrorInterceptorHandler handler,
  ) => handler.next(err);
}
```

> `onRequest` : 요청을 할 때 실행 될 로직 작성
>
> `onResponse` : 응답을 받을 때 실행 될 로직 작성
>
> `onError` : 에러를 받을 때 실행 될 로직 작성