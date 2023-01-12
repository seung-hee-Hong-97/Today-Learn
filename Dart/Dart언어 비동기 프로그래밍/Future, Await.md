# Future

Future 클래스를 사용하여 비동기 프로그래밍을 구현할 수 있습니다.

<br/>

## deplayed 함수

`1번 파라미터` - 지연할 시간: 얼마나 지연을 시킬 것인지 -> Duration object 사용 -> Duration의 파라미터로는 시간을 할당

``` dart
const duration = Duration(days: 1, hours: 8, minutes: 56, seconds: 59,
  milliseconds: 30, microseconds: 10);
print(duration); // 32:56:59.030010
```

`2번 파라미터` - 지연이 끝난 후 실행 될 함수

``` dart
// 사용 예제

void main() {
  // Future를 사용한 변수 선언
  Future<String> name = Future.value('hong');
  Future<int> number = Future.value(3);
  Future<bool> isTrue = Future.value(true);
  
  
  addNumbers(3, 4);
  addNumbers(2, 2);
}

// 파라미터를 더하는 함수 생성
void addNumbers(int num1, int num2) {
  print('계산 시작: $num1 + $num2');
  
  Future.delayed(Duration(seconds: 2), () {
    print('게산 성공: ${num1 + num2}');
  });
  
  print('함수 실행 완료');
}

/* 실행 결과
	계산 시작 3 + 4
  함수 실행 완료
  계산 시작 2 + 2
  함수 실행 완료
  계산 성공: 7
  계산 성공: 4
  
  -> 작성한 코드의 위치 상관 없이 2초 지연 후 실행된 함수에 있는 print 문이 가장 늦게 출력 된 것을 확인 할 수 있음
*/
```

> 코드를 실행할 때 지연된 부분이 있으면 CPU가 먼저 처리할 수 있는 작업부터 하고 그 후에 지연된 작업을 처리하는 것을 확인할 수 있습니다.

<br/>

# Await

await를 사용하기 위해서는 await가 사용되는 함수가 async로 선언되어 있어야 합니다.

또한 await는 Future를 리턴해주는 함수에만 사용할 수 있습니다.

``` dart
// 사용 예제 1
void main() {
  addNumbers(3, 4);
  addNumbers(2, 2);
}

// async / await 사용
void addNumbers(int num1, int num2) async{
  print('계산 시작: $num1 + $num2');
  
  await Future.delayed(Duration(seconds: 2), () {
    print('계산 성공: $num1 + $num2 = ${num1 + num2}');
  });
  
  print('함수 실행완료 $num1 + $num2');
}

/* 실행 결과
  계산 시작: 3 + 4
  계산 시작: 2 + 2
  계산 완료: 3 + 4 = 7
  함수 실행 완료 3 + 4
  계산 완료: 2 + 2 = 4
  함수 실행 완료 2 + 2
*/
```

> async / await를 사용하지 않았을 경우에는 `계산 성공` 부분이 가장 마지막에 실행된 것을 확인할 수 있었습니다.
>
> async / await를 사용하면 await 하단에 있는 `함수 실행 완료` 부분이 await가 실행 된 후에 실행 된 것을 확인할 수 있습니다.
>
> 즉  async / await를 사용하게 되면 먼저 처리 할 수 있는 작업은 하되, await가 붙은 코드 보다 나중에 작성된 코드는 await 코드가 종료 된 후에 실행 되는 것을 확인할 수 있습니다.

<br/>

만약 함수 자체가 실행 종료 된 후에 다음 함수를 실행하고 싶으면 함수에다가 `await` 를 사용하면 됩니다.

``` dart
// 사용 에제 2
void main() async {
  await addNumbers(3, 4);
  await addNumbers(2, 2);
}

/* await는 Future를 리턴해주는 함수에만 사용할 수 있기 때문에 addNumbers 함수의 반환 타입을 Future<void>로
  바꿔준 것을 확인할 수 있습니다. */
Future<void> addNumbers(int num1, int num2) async{
  print('계산 시작: $num1 + $num2');
  
  await Future.delayed(Duration(seconds: 2), () {
    print('계산 성공: $num1 + $num2 = ${num1 + num2}');
  });
  
  print('함수 실행완료 $num1 + $num2');
}

/* 실행 결과
  계산 시작: 3 + 4
  계산 완료: 3 + 4 = 7
  함수 실행 완료 3 + 4
  계산 시작: 2 + 2
  계산 완료: 2 + 2 = 4
  함수 실행 완료 2 + 2
*/
```

> 실행 결과를 살펴보면 `addNumber(3, 4)`가 실행이 끝나면 `addNumbers(2, 2)`가 실행 되는 것을 확인할 수 있습니다.

<br/>

await를 사용한 함수로부터 반환 값을 받을 수도 있습니다.

``` dart
// 사용 예제 3
void main() async {
  final result1 = await addNumbers(3, 4);
  final result2 = await addNumbers(2, 2);
}

Future<int> addNumbers(int num1, int num2) async {
  print('계산 시작: $num1 + $num2');
  
  await Future.delayed(Duration(seconds: 2), () {
    print('계산 완료: $num1 + $num2 = ${num1 + num2}');
  });
  
  print('함수 실행 완료 $num1 + $num2');
  
  return num1 + num2;
}

/* 실행 결과
  계산 시작: 3 + 4
  계산 완료: 3 + 4 = 7
  함수 실행 완료 3 + 4
  계산 시작: 2 + 2
  계산 완료: 2 + 2 = 4
  함수 실행 완료 2 + 2
  result1: 7
  result2: 4
*/
```

> `addNumbers(3, 4)` 함수의 리턴 값인 7을 result 1에 대입
>
> `addNumbers(2, 2)` 함수의 리턴 값인 4를 result 2에 대입