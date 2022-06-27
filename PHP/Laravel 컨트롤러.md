# Laravel 컨트롤러

컨트롤러는 라라벨의 MVC(Model-View-Controller)에서 C에 해당합니다.

컨트롤러는 어플리케이션이 가지고 있는 대부분의 비즈니스 로직을 구현하는 부분이고 라라벨에서 컨트롤러는 라우트에서 지정하고 별도의 컨트롤러 클래스를 두는 것이 일반적인 방식입니다.

기본적으로 컨트롤러 클래스 파일은 `app/Http/Controllers` 경로의 하위에 위치합니다.



## 기본 컨트롤러

라라벨 프레임워크를 설치하여 실행하면 기본적으로 `routes/web.php` 파일에 아래와 같은 코드가 있습니다.

```php
<? php
  use Illuminate\Support\Facades\Route;

  Route::get('/', function () {
    return view('welcome');
  });

?>
```

> 이 코드의 의미는 현재 `/` 에 해당하는 컨트롤러가  `welcome` 뷰를 반환하는 것입니다.

라라벨에서 컨트롤러 클래스를 생성하는 방법은 아래의 명령어를 입력하여 만들 수 있습니다.

``` bash
$ php artisan make:controller <생성할 컨트롤러 이름>

# ex) -> php artisan make:controller HomeController
```

위의 명령어를 입력하여 생성한 컨트롤러 클래스는 `app/Http/Controllers` 디렉토리 안에 `HomeController.php` 파일이 생성됩니다.

기본적으로 라라벨은 `PSR-4 Autoload` 규약을 따르기 때문에 디렉토리 경로와 네임스페이스를 매핑합니다.

``` php
<? php
  namespace App\Http\Controllers;
  
  use Illuminate\Http\Request;

  class HomeController extends Controller {
    public function index() {
      return view('welcome');
    }
  }
?>
```

위와 같이 컨트롤러를 생성하면 `web.php` 파일에서 Route를 다음과 같이 설정할 수 있습니다.

``` php
// web.php

<? php
  use Illuminate\Support\Facades\Route;
  use App\Http\Controllers\HomeController;

  Route::get("/", [HomeController::class, "index"]);

?>
```

> 전체적인 로직은 컨트롤러 클래스에서 작성하고 Route에만 신경쓰면 되는 구조



## 리소스 컨트롤러

리소스 컨트롤러는 Route 구성을 `RESTful` 한 URL 인터페이스로 구성할 수 있습니다.

RESTful한 구조에서는 하나의 리소스에 대해 Route를 사람이 알기 쉽도록 구성합니다. 주로 SPA(Single-Page-Application) 프레임워크와 사용할 때 API Route를 구성할 때 사용하지만 브라우저용으로 구성하는 것도 좋습니다.

리소스 Route는 라라벨의 전통(?)에 따라 **복수형**의 단어를 사용합니다.

리소스 컨트롤러는 아래의 명령어를 사용하여 생성합니다.

``` bash
$ php artisan make:controller TaskController --resource
```

> `--resource` 옵션을 사용하면 모델에 대한 생성, 읽기, 업데이트 및 삭제 (CRUD)를 처리하는 컨트롤러를 빠르게 생성할 수 있습니다.



리소스 컨트롤러를 생성헀다면 아래와 같이 리소스 Route를 생성하고 등록합니다.

``` php
use App\Http\Controllers\TaskController;

Route::resource('tasks', TaskController::class);
```



`resources` 메소드에 배열을 전달하여 한번에 여러 개의 리소스 컨트롤러를 등록할 수도 있습니다.

``` php
Route::resources([
  'tasks' => TaskController::class,
  'tests' => TestController::class,
]);
```



리소스 컨트롤러 생성 및 리소스 Route를 등록했다면 HTTP Method에 따라 Route가 등록되어 있고 그에 따라 RESTful 한 형태로 URL 인터페이스를 확인할 수 있습니다.

``` bash
$ php artisan route:list
```

### 리스트

| Method       | URI               | Name          | Action  |
| ------------ | ----------------- | ------------- | ------- |
| GET \| HEAD  | tasks             | tasks.index   | index   |
| POST         | tasks             | tasks.store   | store   |
| GET \| HEAD  | tasks/create      | tasks.create  | create  |
| GET \| HEAD  | tasks/{task}      | tasks.show    | show    |
| PUT \| PATCH | tasks/{task}      | tasks.update  | update  |
| DELETE       | tasks/{task}      | tasks.destroy | destroy |
| GET \| HEAD  | tasks/{task}/edit | tasks.edit    | edit    |

> `Name` : Route의 이름
>
> `Action` : 해당 Route에 해당하는 컨트롤러



## 리소스 라우트 파라미터

Route에는 파라미터를 받을 수 있는데, 위의 리스트에서와 같이 `{task}`에 해당하는 부분이 파라미터입니다.

```php
/**
* @param int $id
* @return \Illuminate\Http\Response
*/

public function show($id) {
  // code..
}
```

> 위의 코드는 tasks.show에 해당하는 컨트롤러 코드입니다.
>
> 파라미터로 `$id` 가 있는 것을 볼 수 있는데, `GET tasks/14` 를 요청헀다면 파라미터에 14가 할당 됩니다.