# [Laravel] Response 리다이렉트

`리다이렉트 response` 는 `Illuminate\Http\RedirectResponse` 클래스의 인스턴스이며, 사용자가 다른 URL로 리다이렉트 시키기 위한 준비된 헤더를 포함하고 있습니다.

`RedirectResponse` 인스턴스를 활용하기 위한 방법 중 가장 간단한 방법은 `redirect` 헬퍼를 사용하는 것입니다.

```  php
Route::get('/dashboard', function () {
  return redirect('home/dashboard');
});
```



제출된 form 양식이 유효하지 않은 경우와 같이, 사용자를 이전 위치로 리디렉션하려는 경우가 있습니다.

이 때 `back` 헬퍼를 사용하면 됩니다.

``` php
Route::post('/user/profile', function () {
  // 유요하지 않은 request
  
  return back()->withInput();
});
```



## 이름이 지정된 Route로 리다이렉트

`redirect` 헬퍼가 아무런 인자 없이 호출될 때에는, `Illuminate\Routing\Redirector` 인스턴스가 반환되어서 `Redirector` 인스턴스의 메소드를 사용할 수 있습니다.

`route` 메소드를 활용하면 이름이 지정된 Route에 대한 `RedirectResponse` 를 생성할 수 있습니다.

``` php
return redirect()->route('login');
```



만일 인자를 받아야 한다면, `route` 메소드의 두번째 인자로 전달할 수 있습니다.

``` php
return redirect()->route('profile', ['id' => 1]);
```



## 컨트롤러 액션으로 리다이렉트

`action` 메소드를 활용하여 컨트롤러 액션으로 리다이렉트하는 응답을 생성할 수도 있습니다.

컨트롤러와 액션의 이름을 `action` 메소드에 전달하면 됩니다.

``` php
use App\Http\Controllers\UserController;

return redirect()->action([UserController::class, 'idnex']);
```



컨트롤러 Route에 파라미터가 필요한 경우, `action` 메소드의 두번째 인자로 전달하면 됩니다.

``` php
use App\Http\Controllers\UserController;

return redirect()->action(
  [UserController::class, 'profile'], ['id' => 1]
);
```



## 외부 도메인으로 리다이렉트

가끔씩 애플리케이션의 외부 도메인으로 리다이렉트 해야 하는 경우가 있습니다.

`away` 메소드를 활용하여 추가적인 URL 인코딩, 유효성 검사와 확인 과정 없이 `RedirectResponse` 를 만들 수 있습니다.

``` php
return redirect()->away('https://www.google.com');
```



## 세션의 임시 데이터와 함께 리다이렉트

새로운 URL로 리다이렉트 되는 것과 세션에 데이터를 임시적으로 저장하는 것은 통상족으로 동시에 완료됩니다.

`RedirectResponse` 인스턴스를 생성하고 데이터를 세션에 임시저장하는 메소드 체이닝을 할 수 있습니다.

``` php
Route::post('/user/profile', function () {
  // Code...
  
  return redirect('dashboard')->with('status', 'Profile updated');
});
```



## 입력값과 함께 리다이렉트

`withInput` 메소드를 활용하여, 핸재 요청의 입력 데이터를 세션으로 플래시 할 수 있습니다.

일반적으로 사용자에게 유효성 검사 오류가 발생한 경우 수행됩니다.

``` php
return back()->withInput();
```