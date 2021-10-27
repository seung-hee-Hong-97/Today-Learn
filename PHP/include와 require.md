# include와 require

php는 다른 파일에 있는 클래스를 사용하기 위해선, `include`, `require`, `include_once`, `require_once` 를 통해 해당 클래스 혹은 함수가 포함되어 있는 파일을 호출해야 합니다. 

include, require의 사용법과 차이점에 대해 알아보도록 하겠습니다.



### `include와 require의 사용법`

``` php
// include 사용법
  <?php
    include '[불러올 파일 명]';
  ?>
  
// require 사용법
  <?php
    require '[불러올 파일 명]';
  ?>
```

> include_once와 require_once는 위의 코드와 같이 사용법이 같습니다.



자신이 작업 중인 현재의 파일에서 불러오고 싶은 파일들을 위의 코드처럼 불러와서 사용할 수 있습니다.

include와 require 뒤에 `_once` 가 붙으면 한 번만 불러온다는 의미입니다.



### `include와 require의 차이점`

include와 require는 기능적으로는 차이점이 없다고 봐도 무방합니다.

다만 존재하지 않는 파일을 불러오도록 명시하는 등의 경우 에러 표시를 하게 되는데 그 때 출력하는 형태가 다릅니다.



**include**

에러가 발생 했을 경우, 해당 에러에 대한 경고를 발생시킨 후 (`Warning Error`) 나머지 코드를 계속 실행합니다.



**require**

에러가 발생 했을 경우, 해당 에러에 대한 경고를 발생시킨 후 (`Fatal Error`) 나머지 코드의 실행을 중단합니다.



``` bash
차이점을 요약 해보자면, include와 require는 아무런 에러 없이 실행을 마친다면 차이점이 없습니다.

하지만 에러가 발생했을 경우, 처리하는 방식이 다릅니다.

조금 더 엄격하게 처리하는 것이 require이고 비교적 엄격하게 처리하지 않는 것이 include라고 보면 될 것 같습니다.
```

