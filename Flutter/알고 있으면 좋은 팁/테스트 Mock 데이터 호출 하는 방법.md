# 테스트 Mock 데이터 호출하는 방법

Flutter를 통해 개발을 진행하다 보면 데이터가 필요하지만, 당장 서버에서 데이터를 받아올 수 없는 상황이 생길 수 있습니다.

이 때 서버에서 반환받을 데이터를 임의적으로 하드코딩하여 Json 파일로 생성할 수 있습니다.

<br />

## mock 데이터 파일 생성

개발에 필요한 Json 데이터를 만들어줍니다.

``` json
[
  {
    name: 'hong',
    age: 20
  },
  {
    name: 'kim',
    age: 24
  },
  {
    name: 'lee',
    age: 27
  }
]
```

> 이해를 돕기 위해 간단하게 작성한 Mock 데이터 JSON 파일
>
> 파일명은 mock_response_person.json으로 하겠습니다.

생성한 파일은 `test_resource/mock_response_person` 경로에 넣겠습니다.

<br />

## Mock 데이터 파일 호출

데이터를 호출하려는 로직에 아래의 코드를 작성하면 됩니다.

``` dart
String jsonString = await rootBundle.loadString('test_resource/mock_response_person.json');

final response = PersonResponseModel.fromJson(jsonDecode(jsonString));
```

> - rootBundle.loadString( ) 내부에 있는 경로는 Mock 데이터 파일이 있는 경로입니다.
>
> - response의 모델 클래스는 데이터를 매핑할 클래스입니다.