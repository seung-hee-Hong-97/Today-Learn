# Stream

`Stream` 은 Dart 언어에서 기본으로 제공하는 기능이 아니기 때문에 Dart 언어에서 기본으로 제공하는 패키지를 `import` 해와야 합니다. (기본으로 제공하는 기능이 아닌데, 기본으로 제공하는 패키지를 import 하면 사용할 수 있는게 좀 신기하네요...)

`StreamController()`를 불러오면 stream을 사용할 수 있습니다.

<br/>

## Stream과 Future의 차이

`Future`는 함수가 끝날 때가 완료인 순간인데 `Stream` 은 Stream을 닫는 순간이 완료 순간입니다.

Stream을 닫기 전까진 계속해서 값을 받아낼 수 있다는 장점이 있지만 그만큼 복잡합니다.

<br/>

## Stream 기본기

위에서도 언급 했듯이 `Stream`을 사용하기 위해선 `dart:async` 패키지에서 `StreamController`를 불러와서 사용해야합니다.

``` dart
import 'dart:async';

void main() {
  final controller = StreamController();
  final stream = controller.stream;
  
  // 리스닝을 통해 리스너에게 add를 통해서 값을 전달 할 수 있고 여러 개의 값을 전달 할 수도 있습니다.
  final streamListener = stream.listen((val) {
    print('listener 1: $val');
  });
  
  controller.sink.add(1);
  controller.sink.add(2);
  controller.sink.add(3);
}

/* 실행 결과
  listener1: 1
  listener1: 2
  listener1: 3
*/
```

> `controller.sink.add(값)`을 통해 리스너에게 값을 전달 할 수 있습니다.

<br/>

## Broadcast Stream

`.asBroadcastStream()`을 사용해 여러 번 리스닝을 할 수 있는 `Stream`을 만들 수 있습니다.

``` dart
import 'dart:async';

void main() {
  final controller = StreamController();
  final stream = controller.stream.asBroadcastStream();
  
  final streamListener1 = stream.listen((val) {
    print('listener1: $val');
  });
  
  final streamListener2 = stream.listen((val) {
    print('listener2: $val');
  });
  
  controller.sink.add(1);
  controller.sink.add(2);
  controller.sink.add(3);
}

/* 실행 결과
  listener1: 1
  listener2: 1
  listener1: 2
  listener2: 2
  listener1: 3
  listener2: 3
*/
```

> `.asBroadcastStream()`을 통해 여러 번 리스닝 할 수 있는 리소스를 만들어서 위의 코드와 같이 2개의 리스너를 만든 것을 확인할 수 있습니다.
>
> 만일 리스너는 여러 개를 생성해놓고 `.asBroadcastStream()`을 사용하지 않으면,
>
> `Uncaught Error: Bad state: Stream has already been listened to.` 라는 에러가 발생합니다.
>
> 이미 Stream이 listen을 했다라는 의미입니다.

<br/>

## Stream값 변형하기

`where`를 사용해서 홀수만 출력하는 스트림 리스터, 짝수만 출력하는 스트림 리스너를 구현 해보겠습니다.

``` dart
import 'dart:async';

void main() {
  final controller = StreamController();
  final stream = controller.stream.asBroadcastStream();
  
  final streamListener1 = stream.where((val) => val % 2 != 0).listen((val) {
    print('odd Listener: $val');
  });
  
  final streamListener2 = stream.where((val) => val % 2 == 0).listen((val) {
    print('even Listener: $val');
  });
  
  controller.sink.add(1);
  controller.sink.add(2);
  controller.sink.add(3);
  controller.sink.add(4);
}

/* 실행 결과
  odd Listener: 1
  even Listener: 2
  odd Listener: 3
  even Listener: 4
*/
```

> 원래는 모든 값을 출력했던 것에 반면 조건을 만족하는 값만 출력 되는 것을 확인할 수 있습니다.
>
> `where` 뿐만 아니라 Functional Programming에서 사용했던 대다수의 함수들을 사용할 수 있습니다.

<br/>

## Stream 함수

`Future`를 사용하여 함수를 생성 했던 것 처럼 `Stream`을 사용하여 함수를 생성하는 방법을 알아보도록 하겠습니다.

``` dart
import 'dart:async';

void main() {
  calculate(1).listen((val) {
    print('calculate1: $val');
  })
}

// Stream 타입으로 해주려면 async*을 붙혀줘야 합니다.
Stream<int> calculate(int num) async* {
  for (int i= 0; i<5; i++) {
    yield i * num;
  }
}

/* 실행 결과
  calculate1: 0
  calculate1: 1
  calculate1: 2
  calculate1: 3
  calculate1: 4
*/
```

> `Future`, `Stream` 을 사용하여 함수를 생성할 때는 `async`를 붙혀줘야 합니다.
>
> 다만 여기서 차이점은 `Future - async, Stream - async*` 이런 식으로 사용해야 합니다.

<br/>

`async *`을 사용하는 Stream 타입의 함수라도 해도 Future를 사용할 수 있습니다.

``` dart
import 'dart:async';

void main() {
  calculate(2).listen((val) {
    print('calculate(2): $val');
  });
  
  calculate(4).listen((val) {
    print('calculate(4): $val');
  });
}

Stream<int> calculate(int num) async* {
  for (int i = 0; i<5; i++) {
    yield i * num;
  }
  
  // 1초 지연
  await Future.delayed(Duration(seconds: 1));
}

/* 실행 결과
  calculate(2): 0
  calculate(4): 0
  calculate(2): 2
  calculate(4): 4
  calculate(2): 4
  calculate(4): 8
  calculate(2): 6
  calculate(4): 12
  calculate(2): 8
  calculate(4): 16
*/
```

> `async*`가 붙은 Stream 타입의 함수에도 Future를 사용할 수 있는 것을 확인할 수 있습니다.

<br/>

## Yield

`.asBroadcastStream()`을 통해 스트림을 여러 개 생성 했는데, 위의 코드와 같이 함수를 동시에 실행 시키면 번갈아가면서 실행 되는 것을 확인할 수 있습니다.

하나의 함수가 모두 실행이 완료가 되고 그 후에 다음 함수가 실행되도록 할 때 `yield*` 를 사용할 수 있습니다.

``` dart
import 'dart:async';

void main() {
  playAllStream().listen((val) {
    print(val);
  });
}

Stream<int> playAllStream() async* {
  yield* calculate(1);
  yield* calculate(1000);
}

Stream<int> calculate(int num) async* {
  for (int i = 0; i<5; i++) {
    yield i * num;
  }
}

/* 실행 결과
  0
  1
  2
  3
  4
  0
  1000
  2000
  3000
  4000
  -> calculate(1) 함수가 모두 실행된 후에 calculate(1000)이 실행 된 것을 확인할 수 있습니다.
*/
```

> `yield`: 값을 하나하나 순서대로 가져올 때 사용
>
> `yield*`: Stream의 모든 값을 가져올 때까지 기다림
