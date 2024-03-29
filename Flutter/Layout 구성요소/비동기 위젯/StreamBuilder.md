# StreamBuilder

- StreamBuilder는 수시로 바뀌는 값에 사용을 합니다.
- async가 아닌 async*를 사용합니다.
- return이 아닌 yield를 사용합니다.

<br />

## 캐싱 기능

FutureBuilder와 마찬가지로 최초 렌더링 시에 데이터가 없는 경우에만 `snapshot.data`에서 null을 반환하고 빌드가 한 번 실행되어 데이터가 있는 경우, 새롭게 빌드가 되어도 기존에 있는 데이터를 보여주고, 새로운 데이터가 들어왔을 때 바뀐 데이터를 보여주는 방식입니다.

<br />

### FutureBuilder와의 차이점

비동기 데이터를 처리하는 용도로 사용되는 것은 동일하지만, 지속적으로 변경(발행) 되는 값을 처리할 수 있습니다. 

현제 stream의 상태가 어떤지 확인할 수 있는 ConnectionState 상태를 가지고 stream의 상태를 확인할 수 있습니다. 

값이 발행될 때마다 builder가 실행됩니다.  



<br />

## Stream 닫기

`Stream` 을 생성하여 listener를 통해 데이터를 다루고, 비동기 처리를 해줍니다.

stream을 직접 생성하는 경우 해당 stream을 직접 닫지 않는다면 계속해서 stream을 유지하며 메모리 누수를 유발합니다.

따라서 Stream의 사용이 끝났으면 `cancel()` 을 통해 Stream을 닫아줘야 합니다.

하지만 `StreamBuilder` 는 기본적으로 Stream의 종료까지 관리합니다.

<br />

## 에러가 발생한 경우

- FutureBuilder와 마찬가지로 데이터, 에러를 받기만 하면 Connection State는 `done`이 됩니다.
- 다만 에러를 반환 받은 경우 실제 데이터는 Null이 됩니다.

<br />

## 사용예시

``` dart
// StreamBuilder와 AsyncSnapshot에 제네릭 타입으로 반환 타입을 설정할 수 있습니다
StreamBuilder<int>(
  stream: streamNumbers(),
  builder: (BuildContext context, AsyncSnapshot<int> snapshot)
) {
   return Container(
     width: MediaQuery.of(context).size.width,
     child: Column(
       children: [
         Text('Connection State: ${snapshot.connectionState}'),  // 연결 상태 (none, waiting, active, done)
         Text('Data: ${snapshot.data}'),  // 데이터
         Text('Error: ${snapshot.error}'),  // 에러
         ElevatedButton(
           onPressed:(
             setState(() {}),
           ),
           child: Text('SetState Button'),
         ),
       ],
     ),
   );
}


// Stream이기 때문에 async* 사용
// 2초 지연 후 9까지 증가하는 함수
Stream<int> streamNumbers() async* {
  for (i = 0; i < 10; i++) {
    await Future.delayed(Duration(seconds: 2));
    
    yield i;
  }
}
```

> - `최초 렌더링 후 2초 동안` : 데이터는 null, 연결 상태는 waiting
> - `2초 후 데이터 streamNumbers 함수 종료까지` : 데이터는 순차적으로 증가, 연결 상태는 active
> - `함수 종료 후` : 데이터는 마지막 값인 9, 연결 상태는 done