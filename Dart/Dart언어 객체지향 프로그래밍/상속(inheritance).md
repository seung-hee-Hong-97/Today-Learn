# 상속 (Inheritance)

상속이란 부모 클래스의 모든 변수 혹은 함수를 자식 클래스에서도 사용할 수 있도록 하는 것을 의미합니다.

- 부모 클래스는 여러 자식 클래스 한테 상속 가능
- 하지만 자식 클래스는 딱 하나의 클래스만 상속 받을 수 있음

``` dart
// 상속 사용 예제

void main() {
  Person p1 = Person({age: 20, name: 'kim'});
  Boy b1 = Boy(24, 'hong');
  
  b1.printInfo();
}

class Person {
  int age;
  String name;
  
  Person({required this.age, required this.name});
  
  void printInfo() {
    print('Name: ${this.name}, Age: ${this.age}');
  }
}

// Person 클래스를 상속받는 Boy 클래스
class Boy extends Person {
  Boy(int age, String name):super(age: age, name: name);
}
```

> 부모 클래스인 Person을 상속 받았기 때문에, Person 클래스에 있는 함수 및 변수를 선언하지 않고도 사용 가능



## 타입 체크

 인스턴스 생성 후 동일한 인스턴스인지 비교하는 법입니다.

``` dart
// 위의 코드와 이어 갑니다.

void main() {
  print(p1 == b1); // false
  print(p1 is Person); // true
  print(b1 is Boy); // true
}
```

