# RxJS는 무엇인가?

`RxJS` 는 ReactiveExtensions For JavaScript의 약자이고 JavaScript의 라이브러리이고 TypeScript에서도 사용할 수 있다.



`RxJS` 는 이벤트 스트림을 `Observable`이라는 객체로 표현한 후 비동기 이벤트 기반의 프로그램을 작성할 때 도움이 된다.



또한 이벤트 처리를 위한 API로 다양한 연산자를 제공하는 함수형 프로그래밍 기법도 도입이 되어 있기 때문에, 함수형 프로그래밍에 어느정도 익숙한 사람이면 사용할 때 편리할 수 있다.



## 리액티브 프로그래밍

`리액티브 프로그래밍` 은 이벤트 혹은 배열 같은 데이터 스틀미을 비동기로 처리하여 변화에 좀 더 유연하게 반응하는 프로그래밍 기법이다. 

기존의 프로그래밍 방식은 배열과 함수 반환값과 같은 **동기 데이터** 를 처리하는 방식, `Ajax` 통신 결과, 사용자 이벤트와 같은 비동기 데이터 처리 방식이 제각각 있었지만, 리액티브 프로그래밍은 `동기/비동기` 와 상관없이 데이터를 생산하는 것이라면 무엇이든 시간축을 따라 연속적으로 흐르는 형태의 데이터 스트림으로 처리를 한다.

 `리액티브 프로그래밍` 을 활용하여 외부와 통신하는 방식으로는 `pull` , `push` 2가지로 나누어 진다.

### Pull 

외부에서 명령하여 응답을 받고 처리하는 방식이다.

데이터를 가지고 오기 위해서는 계속해서 호출해야 한다.



### Push

외부에서 명령을 기다리지 않고, 응답이 오면 그 때 반응하여 처리하는 방식이다.

데이터를 가지고 오기 위해서는 구독(Subscribe)을 해야 한다.

>  리액티브 프로그래밍은 일반적으로 `Push` 방식을 많이 사용한다.





## RxJS

JavaScript 혹은 TypeScript를 사용하여 코딩을 할 때, 비동기 코드가 많아지면 그만큼 제어의 흐름이 복잡하게 얽혀서 코드를 예측하고 컨트롤하기가 어려워진다.

`RxJS` 는 JavaScript의 비동기 프로그래밍의 문제를 해결하는데 도움을 주고, `Observer` 패턴을 적용한 `Observable` 이라는 객체를 중심으로 동작한다.

`RxJS 라이브러리` 는 `Angular CLI` 를 사용하여 생성한 프로젝트에는 자동으로 포함되어 있지만, 그렇지 않은 경우에는 포함이 되어 있지 않다.

```typescript
import { Observable, Subject, asapScheduler, pipe, of, from, interval, merge, fromEvent } from 'rxjs';
```

> `RxJS` 를 사용할 때 import 시킬 수 있는 것들



```typescript
import { map, filter, scan, tap } from 'rxjs/operators';
```

> `rxjs/operators` 는 pipe 내에서 사용할 수 있는 연산자들을 뜻한다.



## RxJS Observable

`Observable` 은 특정 객체를 관찰하는 `Observer` 에게 여러 이벤트나 값을 보내는 역할을 수행한다.

즉 데이터 스트림을 생성하고 방출하는 객체를 `Observable` 이라고 한다.



`Observer` 는 Observable을 구독하여 방출한 Notification(알림)을 전달받아 사용하는 객체를 뜻한다.

즉 Observable은 연속성을 가지는 데이터를 스트리밍하고, Observer는 연속적으로 보내진 데이터를 받아서 처리를 한다.



`Observer` 에는 총 3가지 메서드가 있다.

### next

Observable 구독자에게 데이터를 전달한다.



### complete

Observable 구독자에게 완료되었다 라는 것을 알린다. 이 때 next 메서드는 더 이상 데이터를 전달하지 않는다.



### error

Observable 구독자에게 에러를 전달한다. 이후에 next 및 complete 이벤트가 발생하지 않는다.

```typescript
// 예시 코드

Observable.create((ob) => {
  try {
    // next 알림(Notification) 방출
    ob.next('one');
  } catch(e) {
    // error 알림(Notification) 방출
    ob.error(e);
  } finally {
    // complete 알림(Notification) 방출
    ob.complete();
  }
}).subscribe(
  // Observable이 방출한 next 알림(Notification)에 반응하는 next 메서드
  (x) => console.log(x);
  
  // Observable이 방출한 error 알림(Notification)에 반응하는 error 메서드
  (err) => console.log(err);

	// Observable이 방출한 complete 알림(Notification)에 반응하는 complete 메서드
  () => console.log('complete');
)
```



### Observer 메서드 표

---

| Observer 메서드 | 설명                                                         | 알림(Notification)내용 |
| --------------- | ------------------------------------------------------------ | ---------------------- |
| next()          | Observable이 방출한 next 타입의 알림(Notification)에 반응하는 콜백 함수 | 값 또는 이벤트         |
| error()         | Observable이 방출한 error 타입의 알림(Notification)에 반응하는 콜백 함수 | 에러 객체              |
| complete()      | Observable이 방출한 complete 타입의 알림(Notification)에 반응하는 콜백 함수 | 없음                   |



<img src="https://poiemaweb.com/img/observable-observer.png" width="400" height="500">



## RxJS Observable 생명주기

### 1. 생성

`Observable.create()`

생성시점에는 어떠한 이벤트도 발생하지 않는다.



### 2. 구독

`Observable.subscribe()`

구독시점에서는 이벤트를 구독할 수 있다.



### 3. 실행

`Observable.next()`

실행시점에서는 이벤트를 구독하고 있는 대상에게 데이터(값)을 전달한다.



### 4. 구독 해제

`Observable.complete()`

`Observable.unsubscribe()`

구독해제 시점에 구독하고 있는 모든 대상의 구독을 종료한다.





## RxJS Subject

RxJS에서는 `Subject` 를 지원한다.

`Subject ` 는 여러 `Observer` 에서 구독을 할 수 있으며, Subject에서 전달하는 값을 Observer들이 전달받는 형태이다.

``` typescript
// 예시 코드

const subject = new Subject();

subject.subscribe(
  (item) => console.log('one', item),
  (err) => console.log(err),
  () => console.log('one complete');
);

subject.subscribe(
  (item) => console.log('two', item),
  (err) => console.log(err),
  () => console.log('two complete');
);

subject.next('item');
subject.complete();

// 출력 값
one item
two item
one complete
two complete
```

