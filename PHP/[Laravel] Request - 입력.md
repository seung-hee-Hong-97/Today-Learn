# [Laravel] Request - 입력



## 모든 입력값 조회

### all

`all` 메서드를 사용하여 들어오는 요청의 모든 입력 데이터를 `array`로 검색할 수 있습니다.

이 메서드는 들어오는 요청이 HTML양식에서 온 것인지 XHR 요청에서 온 것인지에 관계없이 사용 가능합니다.

``` php
$input = $request->all();
```



### collect

`collect` 메서드를 사용하면 들어오는 요청의 모든 입력 데이터를 `collection` 으로 검색할 수 있습니다.

``` php
$input = $request->collect();
```

또한 들어오는 요청 input의 하위 집합을 `collection` 으로 검색할 수도 있습니다.

``` php
$request->collect('users')->each(function ($user) {
  // Code...
})
```



## 입력값 조회

`input` 메서드를 사용하면 HTTP verb에 관계없이 사용자 입력을 조회할 수 있습니다.

``` php
// 1. 기본 형태
$name = $request->input('name');

// 2. 두번째 인자로 기본값을 전달할 수 있습니다. Request에 요청된 입력 값이 존재하지 않을 시 기본값 반환
$name = $request->input('name', 'Kim');

// 3. 배열 입력을 가진 폼에서 동작할 시, 배열에 접근하기 위해 점 표기법 사용 가능
$name = $request->input('products.0.name');
$name = $request->input('products.*.name');

// 4, 모든 입력 값을 연관 배열로 검색하기 위해 인자 없이 input 메소드 호출 가능
$input = $request->input();
```



## 쿼리 스트링에서 입력값 조회

- `input 메소드` : 요청 전체의 payload(쿼리 스트링 포함)에서 값을 조회
- `query 메소드` : 쿼리 스트링에서만 값을 조회 

``` php
// 1. 기본 형태
$name = $request->query('name');

// 2. 찾고자 하는 데이터가 존재하지 않을 시, 메소드의 두번째 인자로 기본값 전달 가능
$name = $request->query('name', 'Kim');

// 3. 전체 쿼리 스트링의 값들을 배열 형태로 반환하려면, query 메소드를 호출할 때 아무런 인자를 전달하지 않음
$query = $request->query();
```



## JSON 입력값 조회

JSON 요청을 애플리케이션에 보낼 때 요청의` Content-type 헤더`가 `application/json`으로 올바르게 설정되어 있다면, `input` 메소드를 통해 JSON 데이터에 엑세스 할 수 있습니다.

점 표기법을 통해 JSON 배열 내에 중첩된 값을 검색할 수도 있습니다.

``` php
$name = $request->input('user.name');
```



## Boolean 입력값 조회

`boolean` 메소드는 `1, "1", true, "true", "on", "yes"` 에 대해서만 true를 반환하고 나머지는 false를 반환합니다.

``` php
$archived = $request->boolean('archived');  // false 반환
$on = $request->boolean('on'); // true 반환
```



## Date 입력값 조회

`date` 메서드를 사용하면 날짜/시간을 포함하는 입력값을 `Carbon` 인스턴스로 검색할 수 있습니다.

요청에 대해 주어진 이름의 입력 값이 없으면 `null` 을 반환합니다.

```php
// 1. 기본 형태
$date = $request->date('date');

// 2. 두번째 인자: 날짜 형식, 세번째 인자: 시간대
$elapsed = $request->date('elapsed', '!H:i', 'Asia/Seoul');
```

> 입력값이 있지만 형식이 잘못된 경우 `InvalidArgumentException` 이 발생합니다.
>
> 따라서 `date` 메소드를 호출하기 전에는 입력값을 검증하는 것이 중요합니다.



## 동적 속성을 통한 입력값 조회

`Illuminate\Http\Request` 인스턴스의 동적 속성을 이용하여 사용자 입력에 엑세스 할 수 있습니다.

애플리케이션의 `form` 중 하나가  `name` 필드를 가지고 있다면, 다음과 같이 엑새스 할 수 있습니다.

``` php
$name = $request->name;
```

> 동적 속성을 사용할 때, Laravel은 먼저 요청 payload안에 있는 파라미터 값을 찾습니다.
>
> 만일, 값이 없다면 Route 파라미터 안에 있는 필드를 찾습니다.



## 입력값의 일부 조회

`only`, `except` 메소드를 활용하여 입력 데이터의 일부분만 조회할 수 있습니다.

두 메소드 모두 하나의 배열 또는 동적인 인자의 목록을 받아들입니다.

``` php
$input = $request->only(['username', 'password']);
$input = $request->only('username', 'password');

$input = $request->except(['id']);
$input = $request->except('id');
```

> `only` 메소드는 요청에서 `key/value` 쌍을 반환합니다.
>
> 만일 현재 요청에서 존재하지 않는 `key/value`는 반환하지 않습니다.



## 입력이 있는지 확인

### has

`has` 메소드를 사용하면 요청에 어떤 값이 존재하는지 확인할 수 있습니다.

현재 요청에서 값이 존재하면 `true` 를 반환합니다.

``` php
// 1. 기본 형태
if ($requset->has('name')) {
  // Code..
}

// 2. has 메소드에 배열이 주어지면, 지정된 모든 값이 존재하는지 확인합니다.
if ($request->has(['name', 'email'])) {
  // Code...
}
```



### whenHas

`whenHas` 메소드를 사용하면 요청에 값이 있을 경우 주어진 클로저(익명함수)를 실행합니다.

```php
// 1. 기본 형태
$request->whenHas('name', function ($input)) {
  // Code...
};

// 2. 두번째 클로저는 요청에 지정된 값이 없으면 실행되는 whenhas 메소드로 전달할 수 있습니다.
$request->whenHas('name', function ($input)) {
  // `name`이라는 값이 있을 경우 실행
}, function () {
  // `name`이라는 값이 없을 경우 실행
};
```



### hasAny

`hasAny` 메소드는 지정된 값이 존재하면 `true`를 반환합니다.

``` php
// name, email이 존재하면 true, 아니면 false
if ($request->hasAny(['name', 'email'])) {
  // Code..
}
```



### filled

`filled` 메소드는 주어진 변수값이 현재 요청에 존재하면 `true`를 반환합니다.

``` php
// name이라는 변수값이 Request에 있다면 true, 아니면 false
if ($request->filled('name')) {
  // Code..
}
```



### whenFilled

`whenFilled` 메소드는 값이 요청에 있고, 그 값이 비어 있지 않은 경우 주어진 클로저(익명함수)를 실행합니다.

``` php
// 1. 기본 형태
$request->whenFilled('name', function ($input) {
  // Code..
});

// 2. 두번째 클로저는 지정된 값이 없는 경우 실행되는 whenFilled 메소드로 전달될 수 있습니다.
$request->whenFilled('name', function ($input) {
  // 'name'이라는 값이 요청에 포함 되어있는 경우 실행 
}, function () {
  // 'name'이라는 값이 요청에 포함 되어있지 않은 경우 실행
}
```



### missing

`missing` 메소드는 주어진 키가 요청에 있으면 `true` 를 반환합니다.

``` php
// name이라는 key가 Request에 있으면 true, 아니면 false
if ($request->missing('name')) {
  // Code...
}
```



## 추가 입력 병합

가끔씩 추가 입력을 요청의 기존 입력 데이터에 수동으로 병합해야 하는 경우가 있습니다.

이 때 `merge` 메소드를 사용하여 병합할 수 있습니다.

``` php
$request->merge(['mock' => 0]); 
```



`mergeIfMissing` 메소드를 사용하면 해당 key가 요청의 입력 데이터 내에 존재하지 않을 경우, 입력값을 요청에 병합하는데 사용할 수 있습니다.

``` php
$request->mergeIfMissing(['mock' => 0]);
// 'mock'이라는 key가 입력 데이터 내에 존재하지 않을 경우 입력 데이터에 병합합니다.
```



## 쿠키 조회

Laravel에서 생성된 모든 쿠키는 인증 코드와 함께 암호화됩니다.

이 말의 의미는 쿠키정보가 클라이언트에 의해 변경되었을 경우 유효하지 않은 것으로 간주된다는 것을 의미합니다.

Request에서 쿠키 값을 가져오기 위해선 `cookie` 메소드를 사용합니다.

``` php
$value = $request->cookie('name');
```

