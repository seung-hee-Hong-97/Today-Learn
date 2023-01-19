# (2) DatePicker 적용 및 상태 관리

<br />

## DatePicker 적용

전체적인 UI는 구현 했으니, 달력 아이콘을 클릭 했을 때 DatePicker를 적용하여 사용자가 날짜를 선택할 수 있도록 해보겠습니다.

추가적으로 모든 코드를 작성하기에는 길어지므로 DatePicker를 적용하는 코드만 적도록 하겠습니다.

``` dart
IconButton(
  iconSize: 50,
  onPressed: () {
    showCupertinoDialog(
        barrierDismissible: true,
        context: context,
        builder: (BuildContext context) {
          return Align(
            alignment: Alignment.bottomCenter,
            child: Container(
              color: Colors.white,
              height: 300,
              child: CupertinoDatePicker(
                mode: CupertinoDatePickerMode.date,
                initialDateTime: selectedDate,
                onDateTimeChanged: (DateTime date) {},
              ),
            ),
          );
        });
       },
       icon: Icon(Icons.calendar_today),
     ),
```

> `showCupertinoDialog` 
>
> - IOS 디자인의 Dialog를 띄우도록 하는 Future 타입의 Constructor
> - context와 builder는 필수로 있어야 함
>
> `CupertinoDatePicker`
>
> - IOS 디자인의 DatePicker를 띄우도록 하는 클래스
> - onDateTimeChanged 콜백 함수는 필수로 있어야 함
>
> `barrierDismissible` : Dialog를 제외한 영역을 터치하면 Dialog가 닫힐 수 있도록 하는 설정 (Boolean 값 넣어주면 됨)

<br />

## DatePicker를 통한 상태 관리

DatePicker를 띄워서 날짜를 선택하는 것까지는 구현이 되었는데 선택한 날짜가 UI에 반영이 되도록 하려면

`StatelessWidget`으로 구현되어 있는 위젯을 `StatefulWidget`으로 변경해주고 상태 관리를 해줘야 합니다.

<br />

### 최상위 위젯에서 상태 관리

프로젝트를 진행하면 상태관리는 필수적인데 규모가 커지면 상태 관리 라이브러리를 사용하겠지만, 라이브러리를 사용하지 않더라도 최상위 위젯에서 상태관리를 하도록 코드를 작성해야 유지보수 및 가독성이 좋아집니다.

따라서 최상위 위젯인 `HomeScreen` 에서 상태 관리를 하도록 하겠습니다.

<br />

현재 날짜와 달력 아이콘을 눌렀을 때 실행될 onPressed 콜백함수를 `_TopArea` 위젯으로 넘겨주겠습니다.

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

class HomeScreen extends StatefulWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  _HomeScreenState createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  // 현재 날짜 지정
  DateTime selectedDate = DateTime.now();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.brown[100],
      body: SafeArea(
        child: Container(
          width: MediaQuery.of(context).size.width,
          child: Column(
            children: [
              // _TopArea 위젯에 현재날짜와 달력 아이콘 클릭 시 실행 될 콜백 함수를 넘겨줌
              _TopArea(
                selectedDate: selectedDate,
                onPressed: onCalendarPressed,
              ),
              _BottomArea(),
            ],
          ),
        ),
      ),
    );
  }

  // 달력 아이콘 클릭 시 실행될 콜백함수 선언
  void onCalendarPressed() {
    DateTime now = DateTime.now();

    showCupertinoDialog(
      context: context,
      barrierDismissible: true,
      builder: (BuildContext context) {
        return Align(
          alignment: Alignment.bottomCenter,
          child: Container(
            color: Colors.white,
            height: 300,
            child: CupertinoDatePicker(
              mode: CupertinoDatePickerMode.date,  // Picker에 날짜만 나오도록 설정
              initialDateTime: selectedDate,  // 초기 날짜 설정
              minimumDate: DateTime(now.year, now.month, now.day), // 최소 날짜보다 이전 날짜 선택 시 picker 초기화
              onDateTimeChanged: (DateTime date) {
                // DatePicker의 DateTime 값이 바뀌면 selectedDate에 할당하면서 상태 변경
                setState(() {
                  selectedDate = date;
                });
              },
            ),
          ),
        );
      },
    );
  }
}

class _TopArea extends StatelessWidget {
  // final 변수 설정
  final DateTime selectedDate;
  final VoidCallback onPressed;

  // Constructor에 상위 위젯에서 념겨준 파라미터 설정
  _TopArea({
    required this.selectedDate,
    required this.onPressed,
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    DateTime now = DateTime.now();

    return Expanded(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          Text(
            'Pass the Test',
            style: TextStyle(
              color: Colors.white,
              fontFamily: 'sunflower',
              fontSize: 50,
            ),
          ),
          Column(
            children: [
              Text(
                '시험 날짜',
                style: TextStyle(
                  color: Colors.white,
                  fontFamily: 'sunflower',
                  fontSize: 30,
                ),
              ),
              Text(
                '${selectedDate.year}.${selectedDate.month}.${selectedDate.day}', // 상태 변화에 따라 값 설정
                style: TextStyle(
                  color: Colors.white,
                  fontFamily: 'sunflower',
                  fontSize: 20,
                ),
              ),
            ],
          ),
          IconButton(
            iconSize: 50,
            onPressed: onPressed,
            icon: Icon(Icons.calendar_today),
          ),
          Text(
            // 설정한 날짜와 현재 날짜를 뺀 값으로 설정
            'D-${
            selectedDate.difference(DateTime(now.year, now.month, now.day)).inDays
            }',
            style: TextStyle(
              color: Colors.white,
              fontSize: 60,
              fontFamily: 'sunflower',
            ),
          ),
        ],
      ),
    );
  }
}
```

<br />

## 구현 결과

<img src="https://user-images.githubusercontent.com/68320595/213405949-8f00f51a-fd48-4594-a773-1ef9877eb689.gif" height="450" />