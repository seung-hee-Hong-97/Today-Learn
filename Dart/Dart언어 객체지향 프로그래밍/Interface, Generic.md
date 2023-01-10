# Interface, Generic

<br/>

## Interface

Dart 언어에서는 다른 언어와 달리 interface 키워드가 따로 존재하지 않습니다.

따라서 class로 생성을 하는데 이 때 class가 아닌 interface로 간주하려면 `implements` 키워드를 통해 하위 클래스를 설정하면 됩니다.

``` dart
void main() {
  Boy boy = Boy('kim');
  Girl girl = Girl('shin');
  
  boy.printName();
  girl.printName();
}

class PersonInterface {
  String name;
  
  PersonInterface(this.name);
  
  void printName() {
    print('이름 $name');
  }
}

class Boy implements PersonInterface {
  // extends를 통해 상속 받는 것과 달리 implements를 사용하면 부모 클래스의 규격을 자식 클래스에서 똑같이 선언해야함
  // 상위 클래스에 있는 변수, 함수를 생성하지 않으면 에러 발생
  String name;
  
  Boy(this.name);
  
  void printName() {
    print('이름 $name');
  }
}

class Girl implements PersonInterface {
  String name;
  
  Girl(this.name);
  
  void printName() {
    print('이름 $name');
  }
}

/* 실행 결과
  kim
  shin
*/
```

> 상속 같은 경우는 속성과 함수들을 자식 클래스에 물려주기 위해서 사용한다면 <br/>
>
> 인터페이스는 어떤 특수한 구조를 강제하기 위해 사용합니다.

<br />

그런데 PersonInterface는 인스턴스로 만들지 말고 인터페이스로 사용하기 위해 만든 클래스인데, 인스턴스로 만들어 버릴 수가 있습니다.

이럴 때 인터페이스를 인스턴스로 만들지 않기 위해 `abstract` 키워드를 사용하면 됩니다.

``` dart
void main() {
  PersonInterface pi = PersonInterface('kim'); // 인스턴스로 만들 수 없기에 에러 발생
}

abstract class PersonInterface {
  String name;
  
  PersonInterface(this.name):
  
  void printName(); // abstract 키워드를 사용하면 함수의 Body 생략 가능
}

class Boy implements PersonInterface {
  // ..
}

class Girl implements PersonInterface {
  // ..
}
```

<br/>

## Generic

타입을 외부에서 받을 때 사용합니다.

generic 타입은 `<>` 을 통해서 선언할 수 있습니다.

``` dart
void main() {
  Student<String> st1 = Lecture('12', 'hong');
  Student<int> st2 = Lecture(123, 'kim');
  
  st1.printInfo();
  st2.printInfo();
}

class Student<T> {
  final T id;
  final String name;
  
  Student(this.id, this.name);
  
  void printInfo() {
    print(id.runtimeType);
  }
}

/* 실행 결과
  String
  int
*/
```

> generic 타입은 < >안에 여러 개가 들어갈 수 있고 타입명은 단일 대문자가 들어가는 것이 암묵적 규칙입니다.
>
> ex) class Student<T, X, Y> 요런식으로 사용