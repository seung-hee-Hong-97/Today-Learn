# 테스트 공통사항 및 test 라이브러리 메소드

Flutter에는 크게 유닛(단위) 테스트, 위젯 테스트, 통합 테스트로 나누어져 있습니다.

<br />

## 공통 사항

테스트를 하려면 가장 먼저 해야 할 것이 `test` 라이브러리를 의존성 추가 하는 것입니다.

[test 라이브러리](https://pub.dev/packages/test)에 접속하여 버전 확인 후 pubspec.yaml에 의존성 추가를 해줍니다.

``` yaml
dev_dependencies:
  test: ^[version]
```

`test 할 것들은 무조건 test 폴더 하위에 위치해야 하고, 파일 명이 반드시 test로 끝나야 합니다.`

Ex) widget_test.dart, counter_test.dart

<br />

Flutter의 가장 상위 폴더에서 터미널로 테스트를 할 수 있습니다.

``` bash
flutter test test/테스트 할 파일 명
```

> test는 반드시 main 함수에서 실행되어야 합니다.

<br />

## test 라이브러리 메소드

### test

테스트에 대한 설명과 실제 테스트 코드를 적습니다.

시간 제한(timeout) 혹은 테스트 환경 (브라우저, OS) 등을 적을 수 있습니다.

<br />

### expect

테스트의 기대값과 실제값을 비교합니다.

test 함수 내부에 작성합니다

<br />

### setup

테스트를 시작하기 전에 설정을 해줍니다.

테스트 단위 하나마다 실행됩니다.

`test()` 함수 하나가 테스트 단위의 하나입니다.

한 파일에 여러 test 함수가 있다면 여러 번 실행됩니다.

<br />

### setupAll

테스트를 시작하기 전에 설정을 해줍니다.

파일 하나당 한번만 실행됩니다.

<br />

### teardown

테스트를 마치고 할 작업을 정해줍니다.

테스트 단위 하나마다 실행됩니다.

<br />

### teardownAll

테스트를 마치고 할 작업을 정해줍니다.

파일 하나당 한번만 실행됩니다.
