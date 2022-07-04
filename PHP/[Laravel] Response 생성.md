# [Laravel] Response 생성



## 문자열 & 배열

모든 `Route`와 `컨트롤러`는 `응답`을 사용자 브라우저로 다시 반환해야 합니다.

Laravel은 `응답`을 반환하기 위한 몇가지 방법을 제공하는데, 가장 기본적인 응답은 `Route` 혹은 `컨트롤러`에서 문자열을 반환하는 것입니다.

Laravel은 자동으로 문자열을 전체 HTTP Response로 변환 시켜줍니다.

``` php
Route::get('/', function () {
  return 'Laravel Example';
});
```



Route나 컨트롤러에서 문자열을 반환하는 것뿐만 아니라 배열을 반환할 수도 있습니다. 그렇다면 Laravel은 자동으로 배열을 JSON Response로 변활 시켜줍니다.

``` php
Route::get('/', function () {
  return [1, 2, 3];
});
```



## Response 객체

일반적으로 Route에서 단순히 문자열 혹은 배열만을 반환하지는 않습니다. `Illuminate\Http\Response` 전체 인스턴스 또는 `views` 을 반환할 수도 있습니다.

Response 객체를 반환하는 것은 Response의 hTTP 상태 코드 혹은 헤더를 변경할 수 있습니다.

``` php
Route::get('/home', function () {
  return response('Laravel Example', 200)->header('Content-Type', 'text/plain');
})
```



## Eloquent 모델 및 컬렉션

`Eloquent`는 Laravel에서 제공하는 ORM(Object Relational Mapping)입니다. DB 테이블에 대응하는 모델의 프로퍼티에 매핑되는 액티브 레코드 ORM입니다.

Route 및 컨트롤러에서 직접 `Eloquent ORM 모델` 및 `컬렉션`을 반환할 수도 있습니다.

그렇게 하면 Laravel은 모델의 `hidden attributes` 를 준수하면서 모델과 컬렉션을 JSON 응답으로 자동으로 변환 시켜줍니다.

``` php
use App\Models\User;

Route::get('/user/{user}', function ($user) {
  return $user;
});
```



## Response에 헤더 추가

대부분의 `response` 메소드는 인스턴의 유연한 생성을 위해 체이닝이 가능합니다.

``` php
return response($content)
  ->header('Content-Type', $type)
  ->header('X-Header-One', 'Header Value1')
  ->header('X-Header-Two', 'Header Value2');
```

> 사용자에게 response를 반환하기 전에 `header` 메소드를 통해 Header를 추가시켜줄 수 있습니다.



``` php
return response($content)
  ->withHeaders([
    'Content-Type' => $type,
    'X-Header-One' => 'Header Value1',
    'X-Header-Two' => 'Header value2',
  ]);
```

> `withHeaders` 메소드를 사용하여 response에 추가하고자 하는 헤더의 배열을 지정할 수 있습니다.



## Response에 쿠키 추가

`cookie` 메소드를 사용하여 `Illuminate\Http\Response` 인스턴스에 쿠키를 첨부할 수 있습니다.

이름, 값 및 쿠키가 이 메소드에 유효한 것으로 간주되어야 하는 시간(분)을 전달해야 합니다.

``` php
return response('Laravel Example')->cookie('name', 'value', $minutes);
```



또한 `cookie` 메소드는 자주 사용되지 않는 몇가지 인자를 더 받을 수 있습니다.

일반적으로 이 인자들은 PHP의 내장된 `setcookie` 메소드에 제공되는 인자들과 동일한 목적과 의미가 있습니다.

```php
return response('Laravel Example')->cookie('name', 'value', $minutes, $path, $domain, $secure, $httpOnly);
```



나가는 응답과 함께 쿠키가 전송되도록 하고 싶지만, 해당 응답의 인스턴스가 아직 없는 경우, `Cookie` Facades를 이용하여 응답이 전송될 때 응답에 첨부할 쿠키를 `대기열` 에 넣을 수 있습니다.

`queue` 메소드를 사용하여 쿠키 인스턴스를 생성하는데 필요한 인수를 입력 받고 이러한 쿠키는 브라우저로 보내기 전에 나가는 응답에 첨부됩니다.

``` php
use Illuminate\Support\Facades\Cookie;

Cookie::queue('name', 'value', $minutes);
```



## 쿠키 인스턴스 생성

전역 `cookie` 헬퍼를 사용하면 나중에 응답 인스턴스에 첨부할 수 있는 `Symfony\Component\HttpFoundation\Cookie` 인스턴스를 생성할 수 있습니다.

이 쿠키는 응답 인스턴스에 연결되지 않는 한 클라이언트로 다시 전송되지 않습니다.

``` php
$cookie = cookie('name', 'value', $minutes);

return response('Laravel Example')->cookie($cookie);
```



## 쿠키 조기 만료

`withoutCookie` 메소드를 통해 쿠키를 만료시켜 쿠키를 제거할 수 있습니다.

``` php
return response('Laravel Exmaple')->withoutCookie('name');
```



Cookie Facades의 `expire` 메소드를 통해 아직 나가는 인스턴스가 없다면 쿠키를 만료시킬 수 있습니다.

``` php
Cookie::expire('name');
```



## 쿠키 암호화

기본적으로 Laravel에서 생성되는 모든 쿠키는 암호화 되고, 서명이 적용되어 클라이언트에서는 수정 및 확인할 수 없습니다.

만일 애플리케이션에 의해서 생성되는 쿠키의 일부분에서 암호화를 비활성화 하려면 아래의 방법을 통해 할 수 있습니다.

1. `app/Http/Middleware` 디렉토리 접속
2. `App\Http\Middleware\EncryptCookies` 미들웨어의 `$except` 속성 사용

``` php
// EncryptCookies.php

protected $except = [
  'cookie_name',
];
```