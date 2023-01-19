# DateTime 및 Duration

DateTime과 Duration을 사용하여 간단한 에제를 통해 학습해도록 하겠습니다.

<br />

## DateTime

Flutter / Dart에서 `날짜 / 시간`과 관련된 클래스입니다.

``` dart
void main() {
  // 1. 현재 날짜 구하기
  DateTime now = DateTime.now();
  print(now);
  
  // 2. 년, 월, 일, 시간 ~~ 구하기
  print(now.year);
  print(now.month);
  print(now.day);
  print(now.hour);
  print(now.minute);
  print(now.second);
  print(now.millisecond);
}

/* 실행 결과
  2023-01-19 11:41:11.831
  2023
  1
  19
  11
  41
  11
  831
*/
```

<br />

## Duration

한 시점에서 다른 시점까지의 차이를 나타냅니다.

``` dart
void main() {
  Duration duration = Duration(seconds: 60);
  
  print(duration);
  print(duration.inDays);
  print(duration.inHours);
  print(duration.inMinutes);
  print(duration.inSeconds);
  print(duration.inMilliseconds);
}

/* 실행 결과
  0:01:00.000000
  0
  0
  1
  60
  60000
*/
```

> Duration의 기본 값은 `int days = 0, int hours = 0, int minutes = 0, int seconds = 0, int milliseconds = 0, int microseconds = 0` 입니다.

<br />

## 날짜 간의 차이

현재 날짜와 특정 날짜를 설정하여 두 날짜 간에 차이를 알 수 있습니다.

``` dart
void main() {
  // 1. 현재 날짜 설정
  DateTime now = DateTime.now();
  
  // 2. 특정 날자 설정
  DateTime specialDays = DateTime(2018, 6, 19);
  
  // 3. 두 날짜 간 비교
  final difference = now.difference(specialDays);
  
  print(difference);
  print(difference.inDays);
  print(difference.inHours);
  print(difference.inMinutes);
  print(difference.inSeconds);
  print(difference.inMilliseconds);
}

/* 실행 결과
  40211:57:31.851000
  1675
  40211
  2412717
  144763051
  144763051851
*/
```

> `difference` : 파라미터로 `DateTime` 을 받고, 날짜 간의 차이를 Duration 형태로 반환합니다.

<br />

## 날짜 지났는지 여부 확인

A라는 날짜를 기준으로 삼고 특정 날짜와 비교하여 지났는지, 안지났는지의 여부를 확인할 수 있습니다.

``` dart
void main() {
  DateTime now = DateTime.now();
  final specialDay = DateTime(2018, 6, 19);
  
  print(now.isAfter(specialDay));  // true
  print(now.isBefore(specialDay));  // false
}
```

> 위의 코드는 이해를 돕기 위해 아주 간단하게 작성했지만 실제 프로젝트 진행 시 많은 날짜 관련 데이터를 비교해야 할 때 유용하게 사용될 수 있습니다.

<br />

## 날짜 더하고 빼기

날짜에 일, 시간, 분, 초 등을 더하고 뺄 수 있습니다.

```dart
void main() {
  DateTime now = DateTime.now();
  
  print(now.add(Duration(hours: 10)));
  print(now.subtract(Duration(hours: 10)));
}

/* 실행결과
	(현재 시간 13:31:49.068)
  2023-01-19 23:31:46.068  <add>
  2023-01-19 03:31:46.068  <subtract>
*/
```

> `add, subtract` 파라미터로 Duration을 넣어주면 해당 값만큼 더하고 뺀 DateTime을 반환합니다.