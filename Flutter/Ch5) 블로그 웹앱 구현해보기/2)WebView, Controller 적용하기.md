# WebView, Controller 적용하기

`외부 플러그인 추가하기` 문서에 있는 내용대로 `webview_flutter` 플러그인을 설치 했다면, 이제 적용을 해보도록 하겠습니다.

<br />

## Webview 적용

우선 Flutter 코드 기본 설정을 하겠습니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('Home Screen'),
      ),
    );
  }
}
```

> 1. 프로젝트 생성 후 기본으로 생성된 코드는 main 함수 제외 모두 제거
> 2. 위의 코드와 같이 설정

<br />

Scaffold 위젯의 body 부분에 Webview를 선언해주면 됩니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Webview(
        initialUrl: 'https://github.com/sejong77/Today-Learn',
      ),
    );
  }
}
```

> 웹사이트인 깃허브 사이트가 정상적으로 앱에 렌더링 된 것을 확인할 수 있습니다.

<img src="https://user-images.githubusercontent.com/68320595/213067908-f06f8778-feb8-410a-99c3-a0a2bfcc897a.png" height="400" />

<br />

## AppBar 추가

AppBar를 추가하기 위해서는 `Scaffold` 안에 `appBar` 를 선언하여 `AppBar` 위젯을 생성해주면 됩니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.black87,
        title: Text('Web App'),
        centerTitle: true, // IOS, Android 모두 Title이 가운데 정렬
      ),
      body: Webview(
        initialUrl: 'https://github.com/sejong77/Today-Learn',
      ),
    );
  }
}
```

> `centerTitle` : AppBar의 Title의 위치는 Android는 왼쪽, IOS는 가운데가 기본 정렬인데 true / false에 따라 플랫폼 상관 없이 위치를 조절할 수 있습니다.

<img src="https://user-images.githubusercontent.com/68320595/213069134-f5ecef39-39c8-49a0-b0fc-b43e2a285dca.png" height="400" />

<br />

## Controller 생성 및 적용

Controller를 통해 Webview를 컨트롤 할 수 있습니다.

홈 버튼을 생성하여 버튼을 클릭 했을 때 위의 이미지 같이 Today-Learn 페이지로 이동하도록 구현해보겠습니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  WebViewController? controller;  // Null 값을 받을 수 있도록 선언
  final homeUrl = 'https://github.com/sejong77/Today-Learn';  // final 키워드 변수에 Url 대입
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.black87,
        title: Text('Web App'),
        centerTitle: true,
        actions: [
          IconButton(
            onPressed: () {
              // Null 체크
              if (controller == null) {
                return;
              }
              
              // !를 붙여줘 controller는 Null이 될 수 없다고 선언
              controller!.loadUrl(homeUrl);
            },
            icon: Icon(Icons.home),
          ),
        ],
      ),
      body: Webview(
        // 웹뷰가 생성될 때 controller를 함수에서 받을 수 있도록 설정
        onWebViewCreated: (WebViewController controller) {
          this.controller = controller;  // WebViewController? controller에 파라미터로 들어온 controller를 대입
        },
        initialUrl: homeUrl,
      ),
    );
  }
}
```

> `icon 사용` :  `Icon(Icons.아이콘명)` 형태로 사용
>
> `actions` : 내부에 선언된 이벤트를 실행
>
> `IconButton` : onPressed (이벤트 실행할 함수), icon(아이콘 위젯) -> 2개의 필수 값을 넣어줘야 함

<img src="https://user-images.githubusercontent.com/68320595/213086362-83b31cb5-63d9-4d2d-9981-f1b706f53d85.gif" height="400" />

<br />

## JavaScript 설정 문제

<img src="https://user-images.githubusercontent.com/68320595/213083250-4107252e-f4cb-4986-aa3b-bb827928c4f0.png" />

앱 환경에서 웹브라우저를 띄울 경우 JavaScript를 앱 내에서 사용할 수 있도록 설정해줘야 위의 이미지와 같은 오류가 발생하지 않고 정상적으로 웹 서비스를 이용할 수 있습니다.

오류를 없애는 방법은 간단합니다.

``` dart
// 전체적인 코드는 생략하고, JavaScript 설정 부분만 작성

body: WebView(
  // ...
  javascriptMode: JavascriptMode.unrestricted,
),
```

> `javascriptMode` : disabled - 허용하지 않음, unrestricted - 허용함
>
> javascriptMode의 기본값은 disabled
>
> 앱에서 웹뷰를 사용할 때는 javascriptMode를 unrestricted로 설정해줘야지만 JavaScript를 사용할 수 있습니다.
