# [Laravel] Response 타입

`response` 헬퍼 함수는 다른 타입의 `Response` 인스턴스를 생성하는데 사용될 수 있습니다.

`response` 헬퍼 함수가 인자 없이 호출되면, `Illuminate\Contracts\Routing\ResponseFactory` contract의 구현체가 반환됩니다.

이 `contract response` 를 생성하는데 몇가지 유용한 메소드가 있습니다.



## View Responses

`view` 메소드를 활용하면 response의 stauts와 header 뿐만 아니라, response 내용으로 view를 반환할 수 있습니다.

``` php
return response()
  ->view('hello', $data, 200)
  ->header('Content-Type', $type);
```

> 사용자가 직접 HTTP 상태 코드 혹은 특정 헤더 값을 전달할 필요가 없다면, 글로벌 `view` 헬퍼 함수를 사용해야 합니다.



## JSON Responses

`json` 메소드를 활용하면 자동으로 `Content-Type` 헤더를 `application/json`으로 설정하고, PHP json_encode 함수를 사용하여 주어진 배열을 JSON 형식으로 변환 합니다.

``` php
return response()->json([
  'name' => 'Kim',
  'state' => 'CA',
]);
```



만약 JSONP response를 생성하려고 하면, `json` 메소드와 `setCallback`을 조합하면 됩니다.

``` php
return response()
  ->json(['name' => 'Kim', 'state' => 'CA'])
  ->withCallback($request->input('callback'));
```



## 파일 다운로드

`download` 메소드를 활용하면 사용자의 브라우저가 주어진 경로에 해당하는 파일을 다운로드 하게 하는 response를 생성합니다.

이 메소드의 인자에 들어가야 하는 것으로는

- 첫번째 인자 : 파일 경로
- 두번째 인자 : 사용자가 다운로드 하는 파일의 이름
- 세번째 인자 : HTTP 헤더의 배열

``` php
return response()->download($pathTofile);

return response()->download($pathTofile, $fileName, $headers);
```



## 파일 Responses

`file` 메소드를 활용하면 파일을 다운로드 하는 대신, 브라우저에 이미지나 PDF와 같은 파일을 표시할 수 있습니다.

``` php
return response()->file($pathToFile);

// 첫번째 인자 : 파일 경로, 두번째 인자 : HTTP 헤더 배열
return response()->file($pathToFile, $headers);
```

