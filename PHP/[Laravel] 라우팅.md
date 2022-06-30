# [Laravel] 기본적인 Routing

기본적인 Laravel Route는 URI와 클로저로 정의할 수 있으며, 복잡한 Routing 설정 파일 없이도 Routing을 정의하고 쉽게 이해 할 수 있는 방법을 제공합니다.

``` php
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view("welcome");
});
```

> `recources/views` 하위에 있는 파일 이름이 `view("파일 이름")`에 들어갑니다.



## 기본 Route 파일

모든 라라벨의 Route는 `routes` 디렉토리 안에 들어 있는 Route 파일에 정의되어 있습니다.

이 파일은 `RouteServiceProvider` 파일에 의해 자동으로 로드됩니다.

`routes/web.php` 파일은 웹 인터페이스를 위한 Route들을 정의합니다. 이 Route들에는 세션 상태와 CSRF 보호와 같은 기능을 제공하는 `web` 미들웨어 그룹이 할당되어 있습니다.

쉽게 말해 `web.php` 에서는 주로 브라우저로 접근하는 일반적인 요청을, `api.php `에서는 API 서비스를 제공할 때 사용합니다.

> RouteServiceProvider 파일 경로 => `app/providers/RouteServiceProvider.php`

```bash
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```



경우에 따라 여러 개의 HTTP 메소드에 응답하는 Route를 등록해야 할 수도 있습니다. 이 때 `match` 메소드 또는 `any` 메소드를 사용하여 모든 HTTP 메소드에 응답하는 Route를 등록할 수 있습니다.

``` bash
Route::match(['get', 'post'], '/', function() {
  // code..
});

Route::any('/', function() {
  // code..
});
```

> 만약 동일한 URI를 공유하는 여러 개의 Route를 정의할 때는 `get`, `post`, `put`, `patch`, `delete`, `options` 메소드를 `any`, `match`, `redirect` 메소드보다 먼저 정의해야 합니다.



## 이름이 지정된 Route

이름이 지정된 Route는 URL의 생성이나 지정된 Route로의 리다이렉션을 편하게 해줍니다. 이름을 지정하게 되면 나중에 Route가 변경되더라도 이름으로만 접근하면 되기 때문에 유지보수성이 향상됩니다.

이름을 지정하기 위해선 `name()` 메소드를 사용하면 됩니다.

``` php
Route::get('/', function() {
  return view('welcome');
})->name('home');
```



그 후에 이름이 있는 Route에 접근하고 싶다면 `route()` 헬퍼함수를 사용하여 접근할 수 있습니다. 또한 두번째 파라미터로 키값 배열을 사용하여 Route 파라미터를 넘길 수도 있습니다.

``` php
// 1. 이름이 있는 Route 접근
route('home');

// 2. 키값 배열까지 전달
Route::get('/user/{id}/profile', function($id) {
  // code..
}) -> name('home');

route('home', ['id' => 1]);
```



## 뷰(View) Route

복잡하지 않은 단순 View를 반환하기만 하면 되는 경우 `Route::view` 메소드를 사용하면 됩니다.

이 메소드를 사용하게 되면 복잡한 Route나 컨트롤러 전체를 정의하지 않아도 됩니다.

`web.php` 파일에 정의된 기본적인 Routing 방식을 살펴보면 `view()` 헬퍼 함수를 사용하여 반환한 것을 볼 수 있는데 아래와 같이 간단하게 반환 할 수 있습니다.

``` php
/* view() 헬퍼함수 사용 방식 */
Route::get('/', function () {
  return view('welcome');
});

/* Route::view() 사용 방식 */
Route::view('/', 'welcome');

/*첫번째 인자: URI, 두번째 인자: 뷰 파일의 이름, 세번째 인자: 뷰에 제공할 데이터들의 배열*/
Route::view('/welcome', 'welcome', ['name' => 'Hong']);
```

> Route::view 메소드의 인자로 라라벨의 예약어인 `view`, `data`, `status`, `headers` 라는 이름은 사용할 수 없습니다.



## Route 그룹

그룹은 여러 개의 Route를 묶을 때 사용합니다. 같은 네임스페이스를 사용하거나, 주소에 같은 접미사가 붙거나 혹은 미들웨어를 사용하게 될 때 Rotue 하나하나에 그것들을 적용시키는 것은 코드가 길어지고 가독성이 떨어지기 때문에 그룹을 만들어서 처리하면 효율적입니다.

중첩된 그룹은 속성을 상위그룹과 지능적으로 `병합`합니다.

### **미들웨어**

미들웨어는 애플리케이션으로 들어온 HTTP 요청을 간편하게 검증하고 필터링 할 수 있는 방법을 제공합니다.

``` php
Route::middleware(['one', 'two'])->group(function () {
  Route::get('/', function () {
    return view('welcome');
  });
});
```

> 배열에 나열된 순서대로 실행됩니다.

###  **prefix**

Prefix 메소드는 그룹 안의 Route에 특정 URI의 접두어로 지정할 때 사용합니다.

``` php
Route::prefix('users')->group(function () {
  Route::get('/', function () {
    return view('welcome');
  });
});
```

> prefix의 파라미터로 들어간 값이 모든 Route URI 앞에 붙습니다.

### **컨트롤러**

그룹이 모두 동일한 컨트롤러를 사용하는 경우 `controller` 메소드를 사용하여 그룹 안에 정의하는 모든 Route에 대해서 공통 컨트롤러를 연결할 수 있습니다.

또한 그룹 안에서 정의하는 Route는 컨트롤러의 메소드 이름만 지정해주면 됩니다.

``` php
use App\Http\Controllers\ExampleController;

Route::controller(ExmapleController::class)->group(function () {
  Route::get('/exmaple/{id}', 'show');
  Route::post('/exmaple', 'data');
});
```



## CSRF (cross-site Request Forgery)

CSRF는 교차 사이트 요청 위조라고 하며 공격 방법의 일종입니다.

어느 한 사이트 또는 페이지가 다른 사람의 권한을 도용하여 마치 그 사람이 요청한 것처럼 다른 페이지를 속이는 것을 뜻합니다.

라라벨에서 CSRF를 막으려면 `csrf_filed()` 혹은 `csrf_token()` 헬퍼함수를 사용하거나 `@csrf`를 사용할 수 있습니다.

``` html
<form method="POST" action="/">
  {{ csrf_field() }}
  <input type="hidden" name="_token" value="{{ csrf_token() }}">
  @csrf
</form>
```

