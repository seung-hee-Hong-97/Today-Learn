# [Laravel] Request와 상호작용



## Request(요청) 액세스

의존성 주입을 통해 현재 HTTP요청의 인스턴스를 얻기 위해선 `Illuminate\Http\Request` 경로에 있는 클로저 혹은 컨트롤러 메소드에서 클래스를 입력해야 합니다.

`들어오는 요청 인스턴스`는 Laravel Service Container에 의해서 자동으로 주입됩니다.

``` php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller {
  
  /**
  * @param \Illuminate\Http\Request  $request
  * @return \Illuminate\Http\Response
  */
  
  public function store(Request $request) {
    $name = $request->input('name');
  }
}
```



Route 클로저에서 `Illuminate\Http\Request`클래스를 입력할 수도 있습니다. Service Container는 실행될 때 들어오는 요청을 클로저에 자동으로 주입합니다.

``` php
use Illuminate\Http\Request;

Route::get('/', function (Request $request)) {
  // Code..
}
```



## 의존성 주입 및 Route 파라미터

컨트롤러 메서드가 Route 파라미터로부터 입력 값을 받아야 한다면, 다른 의존성을 지정한 후에 Route 파라미터를 나열해야 합니다.

``` php
use App\Http\Controllers\UserController;

Route::put('/user/{id}', [UserController::class, 'update']);
```



컨트롤러 메서드를 정의하여 `Illuminate\Http\Request`를 type-hint 하면서 동시에 Route 파라미터 `id`에 접근할 수 있습니다.

``` php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller {
  /**
  * @param \Illuminate\Http\Request  $request
  * @param string $id
  * @return \Illuminate\Http\Response
  */
  
  public function update(Request $request, $id) {
    // Code..
  }
}
```



## Request 경로와 메소드

`Illuminate\Http\Request`인스턴스는 애플리케이션에 유입되는 HTTP 요청을 검사하기 위해 다양한 메소드를 제공하고, `Symfony\Component\HttpFoundation\Request`클래스를 상속합니다. 



### Request 경로 조회

`path` 메소드는 Request의 경로 정보를 반환합니다. 따라서 들어오는 Request가 `http://example.com/test/mock`을 대상으로 한다면 `test/mock`을 반환합니다.

``` php
$uri = $request->path();
```



### Request 경로 검사

`is` 메소드는 들어오는 Request가 특정 패턴에 상응한다는 것을 확인할 수 있습니다.

``` php
if ($request->is('admin/*')) {
  // Code..
}

/* routeIs 메소드를 활용하면 request 요청이 named Route와 일치하는지 확인할 수 있습니다. */
if ($request->routeIs('admin.*')) {
  // Code..
}
```



### Request URL 조회

유입되는 Request의 전체 URL을 조회하기 위해선 `url` 혹은 `fullUrl` 메소드를 사용하면 됩니다.

- `url` : 퀴리 스트링 없는 URL 반환
- `fullUrl` : 쿼리 스트링 포함한 URL 반환

``` php
$url = $request->url();
$fullUrl = $request->fullUrl();

/* 현재 URL에 query string 데이터를 추가하고 싶으면 fullUrlWithQuery 메소드를 호출하면 됩니다. 
   이 메소드는 주어진 query string 변수 배열을 현재 query string과 병합합니다.
*/
$request->fullUrlWithQuery(['type' => 'home']);
```



### Request HTTP 메소드 조회

`method` 메소드는 Request에 대해 HTTP 메소드를 반환합니다.

HTTP 메소드가 특정 문자열에 대응하는 것을 확인하기 위해선, `isMethod` 메소드를 사용합니다.

``` php
$method = $request->method();

if ($request->isMethod('post')) {
  // Code...
}
```



## Request 헤더

`header` 메소드를 사용하면 `Illuminate\Http\Request` 인스턴스에서 요청 헤더를 검색할 수 있습니다. 만일 요청에 헤더가 없으면 `null`이 반환됩니다.

하지만 `header `메소드는 요청에 헤더가 없으면 반환되는 선택적 두번째 파라미터를 가질 수 있습니다.

``` php
$value = $request->header('X-Header-Name');
$value = $request->header('X-Header-Name', 'default');
```



`hasHeader` 메소드는 요청에 지정된 헤더가 포함되어 있는지 확인하는데 사용할 수 있습니다.

``` php
if ($request->hasHeader('X-Header-Name')) {
  // Code..
}
```

> `X-Header-Name` 이라는 헤더가 포함되어 있는지 확인하는 코드



## Content 협상

Laravel은 `Accept` 헤더를 통해 들어오는 요청의 Content 유형을 검사하는 여러 방법을 제공합니다.

- `getAcceptableContentTypes` : 요청에 의해 수락된 모든 Content 유형을 포함하는 배열을 반환

``` php
$contentTypes = $request->getAcceptableContentTypes();
```

- `accepts` : Content 유형의 배열을 허용하고 요청에 의해 content 유형이 수락되면 true, 아니면 false를 반환

``` php
if ($request->accepts(['text/html', 'application/json'])) {
  // Code..
}
```

- `prefers` : 주어진 Content 유형의 배열 중에서 Request가 가장 선호하는 Content 유형을 결정할 수 있습니다.

``` php
$preferred = $request->prefers(['text/html', 'application/json']);
```

> 주어진 Content 유형 중에서 어떠한 것도 Request에 의해 수락되지 않으면 null을 반환함

