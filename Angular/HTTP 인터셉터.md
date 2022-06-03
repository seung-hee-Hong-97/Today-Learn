# HTTP 인터셉터

인터셉터를 활용하면 서버로 보내는 HTTP 요청을 가로채거나, 변환할 수 있습니다. HTTP 요청에 적용된 인터셉터는 HTTP 응답에도 다시 활용할 수 있으며, 인터셉터 여러개가 순서대로 실행될 수 있도록 체이닝할 수 있습니다.

일반적으로 HTTP 요청/응답에 대해 사용자 인증 정보를 확인하고, 로그를 출력하기 위해 사용합니다.

만약 인터셉터를 사용하지 않는다면, 모든 `HttpClient` 메소드가 실행될 때 마다 필요한 작업을 직접 처리해야 합니다.

## 인터셉터 구현

인터셉터를 구현하기 위해선, `HttpInterceptor` 인터페이스를 사용하는 클래스를 정의하고 이 클래스 내부에 `intercept()` 메소드를 선언해야 합니다.

### 인터셉터 구현 예제

``` typescript
// example-interceptor.ts

import { Injectable } from '@angular/core';
import { HttpEvent, HttpInterceptor, HttpHandler, HttpRequest } from '@angular/common/http';

import { Observable } from 'rxjs';

@Injectable()
export class ExampleInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observalbe<HttpEvent<any>> {
    return next.handle(req);
  }
}
```

`intercept` 메소드는 `Observable` 타입으로 HTTP 요청을 받아서 그에 대한 응답을 반환합니다. 개별 인터셉터는 HTTP 요청에 대한 모든 것을 조작할 수 있습니다.

통상적으로, 인터셉터는 요청을 보내거나 응답을 받는 방향을 그대로 유지하기 위해, `HttpHandler` 인터페이스로 받은 `next` 인자의 `handle()` 메소드를 호출합니다.

`intercept()` 와 비슷하게, `handle()` 메소드도 HTTP 요청을 통해 받은 옵저버블을 `HttpEvents` 타입으로 옵저버블로 변환하며, 이 타입이 서버의 최종 응답을 표현하는 타입입니다.

원래 HTTP 요청이나 응답을 조작하지 않고 그대로 통과시키려면 `next.handle()` 을 실행하면 됩니다.

## next 객체

`next` 객체는 체이닝 되는 인터셉터 중 다음으로 실행될 인터셉터를 의미합니다. 인터센터 체인 중 마지막 인터셉터가 받는 `next ` 객체는 `HttpClient` 백엔드 핸들러이며, 이 핸들러가 실제로 HTTP 요청을 보내고 서버의 응답을 첫 번째로 받는 핸들러입니다.

### 인터셉터 적용 예제

인터셉터를 등록하기 위해선 `@angular/common/http` 에서 `HTTP_INTERCEPTORS` 의존성 주입 토큰을 불러와야 합니다.

``` typescript
{ provide: HTTP_INTERCEPTORS, useClass: ExampleInterceptor, multi: true }
```

> `multi: true` 으로 옵션을 설정하면 `HTTP_INTERCEPTORS` 토큰으로 적용되는 인터셉터가 하나만 있는 것이 아닌, 여러 개가 있다는 것을 의미합니다.

해당 프로바이더 설정을 `AppModule` 의 배열에 바로 추가할 수 있습니다. 하지만 인터셉터가 여러 개 있다면, 이 프로바이더 설정을 한 번에 묶어서 사용하는 방법도 좋습니다.

다만 인터셉터를 여러 개 동시 적용한다면, 인터셉터가 실행되는 순서에 유의해야 합니다.

``` typescript
/* app/http-interceptors/index.ts */

import { HTTP_INTERCEPTORS } from '@angular/common/http';
import { ExampleInterceptor } from './example-interceptor';

export const httpInterceptorProviders = [
  { provide: HTTP_INTERCEPTORS, useClass: ExampleInterceptor, multi: true }
];
```

위와 같이 프로파이더를 설정한 후 `AppModule` 파일의 `providers` 배열에 등록합니다.

``` typescript
// AppModule.ts

// ..otherCode

providers: [
  httpInterceptorProviders
],
```

> 이렇게 등록하면 새로운 인터셉터를 추가하여도 따로 수정할 필요가 없습니다.

## 인터셉터 실행 순서

인터셉터는 등록된 순서대로 실행됩니다. 서버로 요청을 보내기 전에 인증 필드를 추가하고 로그로 남겨야 합니다.

이 경우엔 `AuthInterceptor` 서비스를 등록한 후에 `LoggingInterceptor` 서비스를 등록하면 됩니다. 그러면 외부로 향하는 요청이 `AuthInterceptor` 를 거친 후에 `LoggingInterceptor` 로 전달됩니다. 돌아오는 응답은 반대로 `LoggingInterceptor` 를 거쳐 `AuthInterceptor` 로 전달됩니다.

<img src="https://raw.githubusercontent.com/sejong77/Today-Learn/635ad6e866dd3c74bc0f8ca65c740620bde91690/image/interceptor-order.svg">

> 인터셉터를 등록한 후에는 실행 순서를 변경하거나 특정 인터셉터를 건너뛸 수는 없습니다.
>
> 만일 인터셉터를 적용할지 생략할지를 지정하려면 인터셉터 안에 동적으로 로직을 생성해야 합니다.