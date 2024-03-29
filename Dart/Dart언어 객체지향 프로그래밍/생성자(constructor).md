# 생성자 (Constructor)

Constructor는 Class 내부라면 어디든 선언할 수 있습니다.

또한 변수, List, Function 등 다양한 타입의 값들을 받을 수 있습니다.

``` dart
// 사용 예시

void main() {
  Club mu = new Club('MU', ['park', 'rooney', 'vidic']);
  mu.introduce();
  mu.printMembers();
  
  Club mc = new Club('MC', ['halland', 'KDB', 'stering']);
  mc.introduce();
  mc.printMembers();
}

class Club {
  String name;
  List<String> members;
  
  void introduce() {
    print('Hi, My name is ${this.name}');
  }
  
  void printMembers() {
    print(this.members);
  }
  
  // 생성자 선언
  Club(this.name, this.members);
}


/* 출력 결과
Hi, My name is MU
[park, rooney, vidic]
Hi, My name is MC
[halland, KDB, stering]
*/
```

> `this` : this가 속한 Class를 가리킨다는 의미
>
> 위의 코드를 예를 들면 this.name은 Club 클래스의 내부에 있는 변수 name을 뜻합니다.



## Named Constructor

위의 방식처럼 생성자를 선언하는 방법도 있지만, 또 다른 방법도 있습니다.

``` dart
void main() {
  Club rm = Club.fromList([['vj', 'kb', 'md'], 'RM']);
  rm.introduce();
  rm.printMembers();
}

class Club {
  // 위의 코드 포함하여 작성 할 예정
  
  Club.fromList(List values): this.members=values[0], this.name=[1];
}

// 실행 결과는 위와 동일하게 입력한 값을 출력한다.
```



## Final로 변수 선언하기

클래스 내부에 선언된 변수는 `final` 키워드를 사용하지 못하는 몇몇을 제외하고는 모두 `final` 키워드를 사용하는 습관을 들이는게 좋습니다.

``` dart
void main() {
  // 인스턴스 생성
  Person p1 = new Person(20, 'hong');
  p1.name = 'kim'; // 이렇게 임의로 바꾸는 걸 방지하기 위해 final 키워드를 변수 앞에 붙혀주는게 좋음
}

class Person {
  int age;
  String name;
  
  
  // final 붙히면 임의로 인스턴스 값을 바꿀 수 없음
  final int age;
  final String name;
  
  Person(this.age, this.name);
}

```



## const Constructor

Constructor의 모든 파라미터를 const를 붙여서 사용할 수 있을 경우 선언 가능

```dart
void main() {
  Person p1 = Person(20, 'kim');
  Person p2 = Person(20, 'kim');
  
  print(p1 == p2); // false
  
  // const 붙일 경우
  Person p1 = const Person(20, 'kim');
  Person p2 = const Person(20, 'kim');
  
  print(p1 == p2); // true
}

class Person {
  final int age;
  final String name;
  
  const Person(this.age, this.name);
}
```

> 생성자 앞에 `const` 키워드를 붙여 사용할 수 있음
>
> 또한 const를 붙여서 인스턴스를 생성하게 되면 파라미터가 동일하다는 가정하에 비교했을 시 일치합니다.

<br />

## Factory Constructor

Flutter에서 Model 클래스를 만들 때 자주 사용됩니다.

싱글톤 패턴을 통해 새로운 인스턴스를 생성하는 것이 아닌 하나의 객체만 사용하는 형태

Factory Constructor를 사용하게 되면 현재 클래스 뿐만 아니라 현재 클래스를 상속하고 있는 클래스도 인스턴스화 해서 반환할 수 있음

```dart
void main() {
  final parent = Parent(id: 1);
  final child = Child(id: 3);
}

class Parent {
  final int id;
  
  Parent({
    required this.id,
  });
  
  // factory 생성자 (1)
  factory Parent.fromInt(int id) {
    return Parent(id: id);
  }
  
  // factory 생성자 (2)
  factory Parent.fromInt(int id) {
    return Child(id: id);
  }
}

class Child extends Parent {
  Child({
    required super.id,
  });
}
```

> factory 생성자에서 Parent 클래스와 Child 클래스를 모두 반환할 수 있는 것을 확인 했습니다.
>
> factory 생성자가 선언된 클래스를 상속한 클래스도 반환할 수 있습니다.

<br />

### fromJson을 통한 데이터 모델링

Flutter에서 Model 클래스를 생성하고 JSON 타입으로 받아온 데이터를 매핑할 때 factory 생성자를 사용하는 방법

`JSON 데이터 예시`

``` json
[
  {
    'id': '1',
    'name': 'kim',
    'age': 20,
  },
  {
    'id': '2',
    'name': 'lee',
    'age': 24,
  },
  {
    'id': '3',
    'name': 'park',
    'age': 32,
  }
]
```

<br />

`factory 생성자 사용한 데이터 모델링`

``` dart
// 데이터 받아옴
final item = response.data[index];
final pItem = PersonModel.fromJson(
  json: item,
);

// Model 클래스 및 factory 생성자 적용
class PersonModel {
  final String id;
  final String name;
  final int age;
  
  PersonModel({
    required this.id,
    required this.name,
    required this.age,
  });
  
  factory PersonModel.fromJson({
    required Map<String, dynamic> json  // json 데이터의 기본 타입
  }) {
    return PersonModel(
      id: json['id'],
      name: json['name'],
      age: json['age'],
    );
  }
}
```

