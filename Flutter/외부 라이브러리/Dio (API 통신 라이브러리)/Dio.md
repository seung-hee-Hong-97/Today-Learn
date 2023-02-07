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

// 3. options 추가
final response = await Dio().get(URL, options: Options(
  headers: {'authorization' : "Bearer $token"},
));

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

<br />

### Interceptor 사용 예시

토큰 유효성 검사 로직

``` dart
import 'package:flutter_secure_storage/flutter_secure_storage.dart';
import 'package:dio/dio.dart';

class CustomInterceptor extends Interceptor {
  final FlutterSecureStorage storage;
  
  CustomInterceptor({
    required this.storage,
  });
  
  // 만약 Request의 Header에 accessToken: true 라는 값이 있다면
  // 실제 accessToken을 storage에서 가져와서 authorization: Bearer $token 으로 헤더 변경
  @override
  void onRequest(RequestOptions options, RequestInterceptorHandler handler) async {
    if (options.headers['accessToken'] == 'true') {
      // 헤더 삭제
      options.headers.remove('accessToken');
      
      final token = await storage.read(key: 'ACCESS_TOKEN');
      
      // 실제 storage에 저장된 토큰으로 헤더 변경
      options.headers.addAll({
        'authorization': 'Bearer $token',
      });
    }
    
    // RefreshToken도 위의 AccessToken 로직과 동일
    if (options.headers['refreshToken'] == 'true') {
      options.headers.remove('refreshToken');
      
      final token = await storage.read(key: 'REFRESH_TOKEN');
      
      options.headers.addAll({
        'authorization': 'Bearer $token',
      });
    }
    
    return super.onRequest(options, handler);
  }
  
  // 401 에러가 났을 때 (status code)
  // 토큰을 재발급 받는 시도를 하고 토큰이 재발급 되면 다시 새로운 토큰으로 요청
  @override
  void onError(DioError err, ErrorInterceptorHandler handler) async {
    final refreshToken = await storage.read(key: 'REFRESH_TOKEN');
    
    // storage에 저장된 refreshToken이 없다면 에러를 던짐
    // 에러를 던질 때는 handler.rejrect을 사용
    if (refreshToken == null) {
      return handler.reject(err);
    }
    
    // status code가 401인지 여부 확인
    final isStatus401 = err.response?.statusCode == 401;
    
    // 에러가 발생한 요청이 토큰 받급 받으려는 요청인지 여부 확인
    final isPathRefresh = err.requestOptions.path == '/auth/token';  
    
    // 401 에러이고 토큰을 발급 받으려는 요청이 아니라면 해당 조건문 실행
    if (isStatus401 && !isPathRefresh) {
      final dio = Dio();
      
      try {
        // refreshToken을 서버로 보내 새로운 accessToken을 발급 받는 시도
        final response = await dio.post(
          'http://localhost:3000/auth/token',
          options: Options(
            headers: {
              'authorization': 'Bearer $refreshToken',
            },
          ),
        );
        
        final accessToken = response.data['accessToken']; // 위에서 발급받은 accessToken 할당
        final options = err.requestOptions;  // 에러가 발생한 요청 Options 변수에 할당
        
        // 토큰 변경
        options.headers.addAll({
          'authorization': 'Bearer $accessToken',
        });
        
        // 서버에서 새로 받아온 accessToken을 storage에 저장하여 accessToken 갱신
        await storage.write(key: 'ACCESS_TOKEN', value: accessToken);
        
        // 에러를 발생시킨 기존 요청에서 토큰만 변경하여 요청 재전송
        final resp = await dio.fetch(options);
        
        // 에러가 발생한 상태여도 요청이 성공 했다는 것을 반환
        return handler.resolve(resp);
      } on DioError catch(e) {
        return handler.reject(e);
      }
    }
    
    return handler.reject(err);
  }
}
```

