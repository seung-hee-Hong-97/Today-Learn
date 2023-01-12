# Where, Reduce, Fold

<br />

## Where

``` dart
// 사용 예제

void main() {
  List<Map<String, String>> people = [
    {'name': 'kim', 'live': 'seoul'},
    {'name': 'park', 'live': 'seoul'},
    {'name': 'lee', 'live': 'suwon'},
    {'name': 'shin', 'live': 'suwon'}
  ];
  
  final seoul = people.where((x) => x['live'] == 'seoul').toList();
  final suwon = people.where((x) => x['live'] == 'suwon').toList();
  
  print(seoul);
  print(suwon);
}

/* 실행 결과
  [{name: kim, live: seoul}, {name: park, live: seoul}]
  [{name: lee, live: suwon}, {name: shin, live: suwon}]
*/
```

> where 메서드는 조건을 충족하는 요소들을 필터링하여 재구성하는 기능을 합니다.
>
> 이해를 돕자면 JavaScript의 `filter` 메서드와 비슷한 역할을 합니다.

<br />

## Reduce

`reduce` 메서드는 최초에 실행 될 때는 `index 0` 이 `prev` 값이고, `index 1`이 `next` 값입니다.

이후에는 index 0 위치에 있던 prev 값과 index 1 위치에 있던 next를 더한 값이 prev이고 index 2 위치에 있던 값이 next 이런식으로 계속 반복 됩니다.

``` dart
// 사용 에제

void main() {
  List<int> numbers = [1, 3, 5, 7, 9];
  
  final result = numbers.reduce((prev, next) {
    print('-------------');
    print('prev: $prev');
    print('next: $next');
    print('total: ${prev + next}');
    
    return prev + next;
  });
  
  List<String> words = ['My ', 'Name ', 'is ', 'Hong'];
  
  final introduce = words.reduce((prev, next) {
    print('-------------');
    print('prev: $prev');
    print('next: $next');
    print('introduce: ${prev + next}');
    
    return prev + next;
  });
}

/* 실행 결과
  -------------
  prev: 1
  next: 3
  total: 4
  -------------
  prev: 4
  next: 5
  total: 9
  -------------
  prev: 9
  next: 7
  total: 16
  -------------
  prev: 16
  next: 9
  total: 25
  -------------
  prev: My 
  next: Name 
  introduce: My Name 
  -------------
  prev: My Name 
  next: is 
  introduce: My Name is 
  -------------
  prev: My Name is 
  next: Hong
  introduce: My Name is Hong
*/
```

> reduce 메소드는 List 요소들의 타입과 같은 타입을 리턴해야 합니다.
>
> 즉 numbers.reduce인 경우 int 타입을 반환해야 하고, words.reduce인 경우 String 타입을 반환해야 합니다.

<br/>

## Fold

`fold` 메서드는 `reduce` 메서드와 흡사하지만 가장 최초 prev 값이 `index 0` 위치에 있는 값이 아닙니다.

`fold` 메서드의 첫번 째 파라미터로 주어지는 값이 가장 최초 `prev` 값이 됩니다.

``` dart
// 사용 예제 1

void main() {
  List<int> numbers = [1, 3, 5, 7, 9];
  
  final sum = numbers.fold<int>(0, (prev, next) {
    print('-------');
    print('prev: $prev');
    print('next: $next');
    print('total: ${prev + next}');
    
    return prev + next;
  });
}

/* 실행 결과
  -------
  prev: 0
  next: 1
  total: 1
  -------
  prev: 1
  next: 3
  total: 4
  -------
  prev: 4
  next: 5
  total: 9
  -------
  prev: 9
  next: 7
  total: 16
  -------
  prev: 16
  next: 9
  total: 25
*/
```

> 가장 최초 `prev` 값이 `index 0`에 위치한 값인 1이 아니고 fold 메서드의 첫 번째 파라미터로 설정한 값인 0인 것을 확인할 수 있습니다.

<br/>

```dart
// 사용 예제 2

void main() {
  List<String> words = ['My ', 'Name ', 'is ', 'Hong'];
  
  final introduce = words.fold<String>('', (prev, next) => prev + next);
  print(introduce);
  
  final wordLength = words.fold<int>(0, (prev, next) {
    print('--------');
    print('prev: $prev');
    print('nextLength: ${next.length}');
    print('wordLength: ${prev + next.length}');
    
    return prev + next.length;
  });
}

/* 실행 결과
  My Name is Hong

	--------
  prev: 0
  nextLength: 3
  wordLength: 3
  --------
  prev: 3
  nextLength: 5
  wordLength: 8
  --------
  prev: 8
  nextLength: 3
  wordLength: 11
  --------
  prev: 11
  nextLength: 4
  wordLength: 15
*/
```

<br/>

## Fold와 Reduce의 차이

`fold` 메서드는 반환하는 타입을 generic으로 선언하여 요소의 타입이랑 다르게 설정할 수 있습니다.

위의 코드를 살펴보면 `words` 라는 String 타입의 List가 있는데, `fold 메서드의 generic 타입을 int` 로 선언하여 반환하는 타입을 int로 변경 했습니다.

따라서 각 요소의 길이를 모두 더해 반환하는 기능을 구현할 수 있습니다.