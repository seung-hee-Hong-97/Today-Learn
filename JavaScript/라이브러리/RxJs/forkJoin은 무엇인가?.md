# forkJoin은 무엇인가?

`forkJoin` 은 가변 개수의 관찰 기능을 항목을 허용하고 병렬 형태로 구독을 하는 `RxJS`의 연산자 종류 중 하나이다.

제공된 `Observable`이 모두 방출되고 완료 될 때까지 기다리고, 완료가 되면 `forkJoin` 은 각각 마지막으로 보낸 값들을 수집하여 배열 혹은 객체 형태로 내보낸다.

forkJoin의 기본적인 문법은 다음과 같다.

`forkJoin(..args, selector: function): Observable<T>` 형식으로 많이 사용한다.

결과 스트림은 모든 내부 스트림이 완료될 때 한번만 방출된다. 내부 스트림중에 하나라도 완료가 되지 않았다면, 결과 스트림은 방출되지 않으며 만약 내부 스트림에 오류가 생길 경우 결과 스트림에도 오류가 발생하게 된다.

만약 입력 Observable이 제공되지 않으면 (예시: 빈 배열이 전달됨) 결과 스트림은 즉시 완료된다.



<img src="https://rxjs-dev.firebaseapp.com/assets/images/marble-diagrams/forkJoin.png" widh="300" height="500">

 `forkJoin` 은 두 번 이상 방출되지 않으며, 그 후에 완료된다. 



## forkJoin의 동작 순서

### 1단계

모든 입력 Observable에 구독을 한다.



### 2단계

Observable이 값을 방출할 때, 이 Observable과 일치하는 캐시의 값을 재정의 한다.



### 3단계

Observable의 값이 방출 완료가 되면, 다른 Observable이 완료되었는지 확인한다.

모든 Observable이 완료된 후에만, 각 Observable들의 값을 그룹으로 내보낸다.



### 4단계

모든 Observable이 완료가 되면, Observer에게 알림을 보낸다.



### 5단계

Observable들 중에 오류가 발생한 것이 있으면 Observer에게 오류가 발생했다는 알림을 보낸다.



## Promise.all 방식과의 비교

`forkJoin` 은 `Promise.all` 을 사용하는 것과 비슷한 결과를 보여준다.



### forkJoin 사용한 예시

```typescript
import { forkJoin, of } from 'rxjs';

const join = forkJoin(
  of(3),
  of('what?'),
  of(433),
);

join.subscribe(console.log);

// 출력 값
[3, 'what?', 433]
```



### Promise.all 사용한 예시

```typescript
Promise.all([
  Promise.resolve(3),
  new Promise((res, rej) => {
    setTimeout(res, 3000, 'hi')
  }),
  499,
]).then(value => console.log(value));

// 출력 값
[3, 'hi', 499]
```



위의 예시가 `forkJoin` 연산자를 활용하는 기본적인 방법이다.



## 주문 및 병렬 실행

`forkJoin` 연산자는 완료시기에 상관 없이 내부 Observable의 순서를 유지한다.

즉 timeout같은 걸로 일부러 늦게 실행되도록 만들어도, Observable의 순서를 유지시켜 결과는 동기적으로 나오게 된다.

``` typescript
// 예시 코드

import { forkJoin, pipe, of } from 'rxjs';
import { delay } from 'rxjs/operators';

const delayJoin = forkJoin(
  of('Hello').pipe(delay(3000),
  of('Moo').pipe(delay(1000),
  of('Ho').pipe(delay(2000),
);
                 
delayJoin.subscribe(console.log);

// 출력 값
[ 'Hello', 'Moo', 'Ho' ]
```

> 코드로만 봤을 때는 [ 'Moo', 'Ho', 'Hello' ] 라는 결과 값이 나와야 할 것 같지만 forkJoin은 Observable의 순서를 유지하기 때문에 위의 결과가 나오게 된다.





## forkJoin을 사용한 대체 출력 옵션

기본적으로 `forkJoin` 은 위의 예시처럼 배열형태로 결과를 생성하고 출력한다.

`RxJs 6.5` 버전 이상부터는 쉼표로 구분 된 인수 대신에 `forkJoin` 에 대한 인수로 Object Map을 제공하여 결과를 Object 형식으로 생성할 수도 있다.

``` typescript
// 예시 코드

const objectJoin = forkJoin({
  Moo: of('Moo'),
  Ya: of('Ya'),
  Ho: of('Ho'),
});

objectJoin.subscribe(console.log);

// 출력 값
{ Moo: "Moo", Ya: "Ya", Ho: "Ho"}
```

> forkJoin() 의 소괄호 안에 중괄호를 넣고, key-value 형식으로 작성을 하면 Object 형식으로 출력이 된다.
>
> 배열 형태 혹은 Object 형태 어떤 것을 사용해도 무방하지만, 일관성을 유지하지 않는 방식은 좋지 않다.





## 참고 자료

- [RxJS 공식 사이트](https://www.learnrxjs.io/learn-rxjs/operators/combination/forkjoin)

