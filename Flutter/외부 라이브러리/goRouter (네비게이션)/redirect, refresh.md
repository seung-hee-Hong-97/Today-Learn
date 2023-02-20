# Redirect, Refresh

페이지 네비게이션을 할 때 마다 설정한 로직을 실행하거나 상태가 변경되었을 때 redirect를 실행하는 GoRouter의 핵심기능

<br />

## redirect

페이지 네비게이션을 할 때 마다 설정한 로직을 실행

``` dart
GoRouter(
  // ...
  redirect: (BuildContext context, GoRouterState state) {
    // 로직 작성
  },
)
```

<br />

## refreshListenable

- 상태가 바뀌었을 때 마다 redirect를 재 실행 함
- redirect 같은 경우 네비게이션이 될 때만 실행되기 때문에 토큰이 만료되는 상황과 같이 네비게이션이 일어나지 않아도 redirect가 실행되어야 할 때 필요함
- ChangeNotifier의 `notifyListeners()` 함수를 통해 상태 변경에 대한 리스닝을 함

``` dart
GoRouter(
  // ...
  redirect: ...
  refreshListenable: // ChangeNotifier 클래스를 상속받은 클래스
)
```

<br />

## 사용 예제

유저 정보가 있는 경우 `홈 ('/')` 으로 리다이렉트, 유저 정보가 없는 경우 `로그인 ('/login')` 페이지로 리다이렉트 하는 로직 작성

`flutter_riverpod`, `go_router` 라이브러리를 사용하여 작성

<br />

`UserModel.dart`

``` dart
class UserModel {
  final String name;
  
  UserModel({
    required this.name,
  });
}
```

<br />

`AuthProvider.dart`

``` dart
// 라우터 관련 로직 Provider로 생성
final routerProvider = Provider<GoRouter>((ref) {
  final authStateProvider = AuthNotifier(ref: ref);
  
  return GoRouter(
    initialLocation: '/login',  // 초기 페이지 경로 설정
    redirect: authStateProvider._redirectLogic,  // 네비게이션 될 때 마다 실행될 로직 설정
    refreshListenable: authStateProvider,  // authStateProvider의 상태가 변경되면 실행될 로직 설정
    routes: authStateProvider._routes,  // 페이지 경로 설정
  );
});

class AuthNotifier extends ChangeNotifier {
  final Ref ref;
  
  AuthNotifier({
    required this.ref,
  }) {
    ref.listen<UserModel?>(userProvider, (previous, next) {
      // 이전 데이터와 다음 데이터가 다르면 조건문 실행 => 유저 정보에 대한 상태가 변경되면 redirect 로직 실행
      if (previous != next) {
        notifyListeners();
      }
    });
  }
  
  // redirect 로직
  String? _redirectLogic(BuildContext context, GoRouterState state) {
    final user = ref.read(userProvider);
    final loggingIn = state.location == '/login';  // 현재 페이지가 로그인 페이지인지 확인
    
    // 유저 정보가 없고, 로그인 하려는 중이 아니라면 로그인 페이지로 이동
    if (user == null) {
      return loggingIn ? null : '/login';
    }
    
    // 유저 정보가 있고 로그인 페이지라면 홈으로 이동
    if (loggingIn) {
      return '/';
    }
    
    // 위의 2가지 조건에 해당 안되면 정상적으로 페이지 네비게이션 되도록 null 반환
    return null;
  }
  
  // 페이지 라우트 리스트
  List<GoRoute> get _routes => [
    GoRoute(
      path: '/',
      builder: (_, state) => HomeScreen(),
    ),
    GoRoute(
      path: '/login',
      builder: (_, state) => LoginScreen(),
    ),
  ];
}

// 로그인 / 로그아웃에 대한 데이터를 Provider로 생성
final userProvider = StateNotifierProvider<UserStateNotifier, UserModel?>((ref) => UserStateNotifier());

class UserStateNotifier extends StateNotifier<UserModel?> {
  UserStateNotifier() : super(null);
  
  // 로그인 할 경우 state에 유저 이름 설정, 로그아웃 할 경우 state를 null로 변경
  login({
    required String name,
  }) {
    state = UserModel(name: name),
  }
  
  logout() {
    state = null;
  }
}
```