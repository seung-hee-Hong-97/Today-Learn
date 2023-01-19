# Expanded, Flexible 위젯

Expanded, Flexible 위젯은 사용할 때 주의할 점이 있습니다.

`Row`,  `Column` 위젯의 children 속성 안에 사용해야 합니다. 그렇지 않으면 버그가 발생합니다.

<br />

## Expanded 위젯

`Flexible` 위젯의 fit 옵션이 `FlexFit.tight`로 고정된 위젯입니다.

Row, Column 내부의 Children이 차지하는 공간을 가득 채우도록 할 수 있습니다.

Flexible과 동일하게 남은 공간에 대한 비중을 설정할 수 있지만 Row, Column의 자식 위젯들이 화면을 가득 채우도록 강제합니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Container(
          color: Colors.black,
          child: Column(
            children: [
              Expanded(
                flex: 2,
                child: Container(
                  color: Colors.red,
                  width: 50,
                  height: 50,
                ),
              ),
              Expanded(
                child: Container(
                  color: Colors.orange,
                  width: 50,
                  height: 50,
                ),
              ),
              Expanded(
                child: Container(
                  color: Colors.yellow,
                  width: 50,
                  height: 50,
                ),
              ),
              Expanded(
                child: Container(
                  color: Colors.green,
                  width: 50,
                  height: 50,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

> `flex`  
>
> - 기본 값은 1이며, int 타입이기 때문에 정수 값만 설정할 수 있습니다.
> - 값이 커질 수록 공간을 차지하는 비율이 늘어납니다.

<img src="https://user-images.githubusercontent.com/68320595/212822585-89477b9c-d2f2-488b-896a-65e02037d3d9.png" height="400" />

<br />

## Flexible

`Flexible` 위젯은 Row나 Column의 여유 공간을 활용할 수 있습니다.

즉 Children 내부에 선언된 위젯들이 차지하는 영역을 비율별로 나눌 수 있는 기능을 제공하는데 자식 위젯이 주축을 기준으로 화면을 가득 채우도록 하는 `Expanded` 위젯과 달리 자식 위젯의 원래 크기만큼만 영역을 차지합니다.

`flex` 속성을 통해 차지하는 비중을 조절할 수 있습니다.

``` dart
void main() {
  runApp(MaterialApp(
    home: HomeScreen(),
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: Container(
          color: Colors.black,
          child: Column(
            children: [
              Flexible(
                flex: 2,
                child: Container(
                  color: Colors.red,
                  width: 50,
                  height: 50,
                ),
              ),
              Flexible(
                flex: 2,
                child: Container(
                  color: Colors.red,
                  width: 50,
                  height: 50,
                ),
              ),
              Expanded(
                flex: 2,
                child: Container(
                  color: Colors.red,
                  width: 50,
                  height: 50,
                ),
              ),
              Expanded(
                flex: 2,
                child: Container(
                  color: Colors.red,
                  width: 50,
                  height: 50,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

> 위의 코드에서 자식 위젯의 너비와 높이는 각각 50입니다.
>
> 따라서 Flexible 위젯의 flex 값을 증가시킬 수록 폐기 처리되는 영역이 늘어납니다.

<img src="https://user-images.githubusercontent.com/68320595/212832765-bba00108-b04a-4618-b8dd-520086609b1a.png" height="300" />

<br />

Flutter로 프로젝트 진행 하면서 UI 작업을 할 때 Flexible 보다는 Expanded의 사용빈도가 높습니다.