# Static

static 키워드는 인스턴스에 귀속되지 않고 클래스에 귀속됩니다.

<br/>

## 귀속

``` dart
void main() {
  Person p1 = Person('hong');
  p1.name = 'kim'; // 인스턴스 귀속
  
  Person p2 = Person('park');
  
  Employee.building = "삼성동 빌딩";
  
  p1.printInfo();
  p2.printInfo();
}

class Person {
  String name;
  static String? building;
  
  Person(this.name);
  
  void printInfo() {
    print('이름: ${this.name}, 빌딩: ${building}');
  }
}

/* 실행 결과
  이름: kim, 빌딩: 삼성동 빌딩
  이름: park, 빌딩: 삼성동 빌딩
*/
```

> p1, p2에 각각 인스턴스가 생성되었기 때문에 p1의 name을 kim으로 바꿨다고 해서 p2가 영향을 받지 않습니다.
> 왜냐? 각각의 인스턴스에 귀속이 되었기 때문입니다. <br/>
>
> static이 붙은 building은 클래스에 종속되기 때문에 인스턴스를 생성하지 않고 Employee. 으로 호출할 수 있다.
> 그렇기 때문에 Employee.building = "삼성동 빌딩" 이런 식으로 설정하면 p1, p2 인스턴스 모두 Person이라는 클래스이기 때문에 building 값은 모두 삼성동 빌딩으로 적용 됩니다.