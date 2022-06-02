# 백엔드와 HTTP 통신하기

프론트엔드 애플리케이션은 통상적으로 데이터를 받아오거나 업로드 하기 위해 서버와 HTTP 프로토콜로 통신합니다.

Angular에서는 서버와 통신을 할 때 클라이언트 측 HTTP API인 `@angular/common/http`  패키지로 제공되는 `HttpClient`  서비스를 활용할 수 있습니다.

### HTTP 클라이언트 서비스가 제공하는 기능

- 요청을 보내고 응답을 받을 때, 응답 객체에 타입을 지정할 수 있음
- 에러를 스트림으로 처리
- 테스트를 적용하기 용이함
- 요청과 응답을 가로채서 다른 작업 가능



## 서버와 통신할 준비하기

서버와 통신할 때 사용하는 `HttpClient` 를 사용하기 위해선 먼저 Angular의 `HttpClientModule` 을 로드해야 합니다.

이 모듈은 보통 `AppModule.ts` 파일에 등록합니다.

``` typescript
// AppModule.ts

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  bootstrap: [AppComponent]
})

export class AppModule { }
```

> `HttpClientModule`은 `BrowserModule`을 등록한 뒤에 등록합니다.

모듈을 등록하고 나면 Service 클래스에서 `HttpClient` 서비스를 의존성 주입할 수 있습니다.

``` typescript
// ExampleService.ts

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable()
export class ExampleService {
  constructor (private httpClient: HttpClient) { }
}
```

> `HttpClient` 서비스는 모든 동작에 대해 옵저버블을 활용합니다.

## 서버에 데이터 보내기

`HttpClient` 서비스를 사용하여 서버에 데이터를 요청할 때 사용하는 HTTP 메소드가 `PUT`, `POST`, `DELETE` 라면 서버에 추가 데이터를 보낼 수 있습니다.

### POST 요청 보내기

일반적으로 POST 메소드는 폼을 제출할 때도 사용하며, DB에 데이터를 추가할 때 많이 사용합니다.

``` typescript
// ExampleService.ts

loadExampleData(data: MockData): Observalbe<MockData> {
	return this.httpClient.post<MockData>(this.exUrl, data)
    .pipe(
      catchError(this.handleError('loadExampleData', data))
    );
}
```

>  `HttpClient.post()` 메소드는 `get()` 메소드와 비슷합니다. 서버로부터 받아올 데이터의 타입을 제니릭으로 지정한 뒤, 첫 번째 인자로 서버 API의 URL을 받습니다.

### POST 요청에 대한 응답 받기

``` typescript
// ExampleComponent.ts

this.exampleService.loadExmapleData(data).subscribe((res) => {
  this.datas.push(res);
});
```

> 서버에서 받아온 데이터를 `datas` 라는 배열에 추가하여 새로운 목록을 화면에 표시할 수 있습니다.