# go_router

라우터 API를 사용하여 여러 화면 사이를 네비게이션 하기 위해 URL 기반 API를 제공하는 Flutter용 선언적 라우팅 라이브러리

URL 패턴을 정의하고, URL을 사용하여 탐색, 딥 링크 처리 등 기타 여러 탐색 관련 시나리오를 처리할 수 있습니다.

<br />

## 의존성 추가

[go_router](https://pub.dev/packages/go_router)를 pubspec.yaml 파일에 추가 후 pub get을 해줍니다.

``` yaml
dependencies:
  go_router: ^[version]
```

<br />

## 기본 설정

go_router 라이브러리를 사용하여 라우팅을 전역적으로 적용하려면 최상단인 `main.dart`에 라우팅 설정을 해줘야 합니다.

1. `GoRouter` 클래스를 생성하여 내부에 `initialLocation`, `routes` 를 선언
   - `initialLocation` : 최초 라우팅 경로
   - `routes` : 리스트 형태이고 `GoRoute`를 통해 경로 및 렌더링 할 페이지 설정

<br />

2. `GoRoute` 내부에 경로 및 렌더링 할 페이지 설정
   - `path` : 페이지 경로
   - `builder` : 렌더링 할 페이지 설정

<br />

3. `MaterialApp.router()`를 통해 내부에 라우팅 관련 함수 설정

``` dart
// main.dart

void main() {
  runApp(_App());
}

class _App extends StatelessWidget {
  const _App({Key? key}) : super(key: key);

  // 라우팅 설정
  GoRouter get _router => GoRouter(
        initialLocation: '/',  // 초기 라우팅 경로 설정
        routes: [
          GoRoute(
            path: '/',
            builder: (context, state) => HomeScreen(),
          ),
        ],
      );

  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      routerConfig: _router,
    );
  }
}
```

<br />

## routes 설정 방식

routes를 설정할 때 2가지 방식이 있습니다.

1. 라우트를 중첩해서 설정하는 방식

``` dart
routes: [
  GoRoute(
    path: '/',
    builder: (context, state) => HomeScreen(),
    routes: [
      GoRoute(
        path: 'one',
        builder: (context, state) => OneScreen(),
        routes: [
          GoRoute(
            path: 'two',
            builder: (context, state) => TwoScreen(),
          ),
        ],
      ),
    ],
  ),
],
```

> - `path` 를 설정할 때 `/`를 안붙혀줘도 됩니다.
> - 라우팅 경로가 `http://..../one/two` 형태로 설정됩니다.
> - 라우팅을 중첩해서 설정했기 때문에 페이지 스택이 중첩되어 쌓입니다.

<br />

2. 라우팅을 중첩하지 않고 설정하는 방식

``` dart
routes: [
  GoRoute(
    path: '/',
    builder: (context, state) => HomeScreen(),
  ),
  GoRoute(
    path: '/one',
    builder: (context, state) => OneScreen(),
  ),
  GoRoute(
    path: '/two',
    builder: (context, state) => TwoScreen(),
  ),
]
```

> - `path` 를 설정할 때 각각의 경로가 독립적이므로 `/`를 붙혀줘야 됩니다.
> - 라우팅 경로가 `http://.../one`, `http://...../two` 형태로 설정됩니다.
> - 라우팅을 중첩하지 않고 독립적으로 설정했기 때문에 페이지 스택이 중첩되지 않습니다.

<br />

## 페이지 이동

`go`, `goNamed`, `pop` 등이 있습니다.

`push`, `pushNamed`와 같은 명령형 탐색도 있지만 권장하는 방식은 아닙니다.

<br />

### go

현재의 페이지를 대체하는 새로운 페이지로 새롭게 렌더링이 되는 방식

파라미터에 이동하려는 페이지의 path를 입력

``` dart
// go
context.go('path 입력');
```

<br />

### goNamed

go와 같은 방식이나, 파라미터에 name을 입력하는 것이 차이점

`GoRoute `에 name 설정

``` dart
routes: [
  GoRoute(
    path: '/one',
    name: 'one',  // name 설정
    builder: (context, state) => OneScreen(),
  ),
],

// routeName 값을 외부에서 받아서 사용하는 방법
class OneScreen extends StalessWidget {
  static String get routeName => 'one';
}

context.goNamed(OneScreen.routeName);

// routeName 값을 직접 입력하는 방법
context.goNamed('one');
```

<br />

### pop

뒤로가기

``` dart
context.pop();
```

<br />

## 에러 발생 시 라우팅

라우터에 등록되지 않은 경로로 페이지 라우팅을 했을 때 핸들링하는 방법입니다.

``` dart
GoRouter(
  // ....
  errorBuilder: (context, state) {
    return ErrorScreen();  // 에러가 발생 했을 경우 띄워즐 페이지 위젯을 작성
  },
);
```

<br />

## 현재 페이지 경로 확인하기

``` dart
final router = GoRouter.of(context);

print(router.location);
```