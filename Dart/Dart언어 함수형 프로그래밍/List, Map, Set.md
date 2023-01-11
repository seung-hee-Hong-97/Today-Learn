# List, Map, Set

<br/>

## List

map 메서드를 활용하는 예제

``` dart
void main() {
  List<String> names = ['hong', 'kim', 'park'];
  
  final helloNames = names.map((x) => 'hi $x');
  print(helloNames);
  
  print(names == names);
  print(names == helloNames);
}

/* 실행 결과
  (hi hong, hi kim, hi park)
  true
  false
*/
```

> `map` 메서드를 통해 만든 List는 새로운 List이기 때문에 비교를 했을 경우 false 입니다.

<br/>

## Map

map 메서드를 활용하는 예제

``` dart
void main() {
  // 1. Map 생성
  Map<String, String> person = {
    'Hong': '홍',
    'Kim': '김',
    'Park': '박',
  };
  
  // 2. Map 전체에 MapEntry를 사용하여 map 메서드 적용
  final test1 = person.map((key, value) => MapEntry('EN $key', 'KO $value'));
  print(test1);
  
  // 3. key에만 map 메서드 적용
  final keys = person.keys.map((x) => 'EN $x').toList();
  print(keys);
  
  // 4. value에만 map 메서드 적용
  final values = person.values.map((x) => 'KO $x').toList();
  print(values);
}

/* 실행 결과
{EN Hong: KO 홍, EN Kim: KO 김, EN Park: KO 박}
[EN Hong, EN Kim, EN Park]
[KO 홍, KO 김, KO 박]
*/
```

<br/>

## Set

map 메서드 활용 예제

``` dart
void main() {
  // 1. Set 생성
  Set<String> alphabet = {'a', 'b', 'c', 'd', 'a'};
  
  // 2. map 메서드 적용
  final test = alphabet.map((x) => 'EN $x').toSet();
  print(test);
}

/* 실행 결과
  {EN a, EN b, EN c, EN d}  ==> 중복되는 값인 a는 하나만 출력된 것을 확인
*/
```