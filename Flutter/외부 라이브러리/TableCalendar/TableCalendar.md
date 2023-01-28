# TableCalendar

고도로 사용자 정의가 가능하고 기능이 풍부한 Flutter용 캘린더 위젯 라이브러리 입니다.

앱을 만들 때 달력이 필요한 경우가 많은데, 직접 달력 기능을 구현하려면 생각보다 까다롭고 시간도 많이 소요되기 때문에 매우 유용한 라이브러리 위젯입니다.

<br />

## 라이브러리 의존성 추가

[pub.dev](https://pub.dev/packages/table_calendar)에 접속하여 table_calendar를 복사한 뒤, pubspec.yaml 파일의 `dependencies` 하위에 추가한 뒤 pub get을 눌러 반영해줍니다.

``` yaml
dependencies:
  table_calendar: ^version
```



<br />

## 라이브러리 특징

- 광범위하지만 사용하기 쉬운 API
- 사용자 지정 가능한 스타일로 사전 구성된 UI
- 무제한 UI 디자인을 위한 맞춤형 선택적 빌더
- 로케일 지원
- 범위 선택 지원
- 다중 선택 지원
- 동적 이벤트 및 공휴일
- 수직 자동 크기 조정 - 콘텐츠에 맞추거나 뷰포트를 채웁니다.
- 다양한 달력 형식(월, 2주, 주)
- 수평 스와이프 경계(첫날, 마지막 날)

<br />

## 기본 설정

**`TableCalendar`**는 다음 `firstDay`을 제공해야 합니다 .`lastDay` `focusedDay`

- `firstDay` : 달력에서 사용할 수 있는 첫 번째 날짜입니다. 사용자는 이전 날짜에 액세스할 수 없습니다.
- `lastDay` : 달력에서 사용할 수 있는 마지막 날입니다. 사용자는 이후 날짜를 액세스할 수 없습니다.
- `focusedDay` : 현재 목표 날짜입니다. 이 속성을 사용하여 현재 표시되어야 하는 월을 결정합니다.

``` dart
TableCalendar(
  firstDay: DateTime(2023, 1, 28),
  lastDay: DateTime(2023, 2, 28),
  focusedDay: DateTime.now(),
)
```

> 위의 값들은 전부 필수 값이며, 타입은 `DateTime` 입니다.

<br />

## 캘린더 Header 스타일링 (headerStyle: HeaderStyle())

TableCalendar 위젯에 위의 3가지 필수값을 넣고 렌더링 해보면 헤더 영역에 `지난 날짜, 다음 날짜로 이동할 수 있는 화살표 버튼`, `년월 표시하는 텍스트`, `Format 버튼` 으로 이루어져 있습니다.

`headerStyle: HeaderStyle(스타일링)` 형식으로 되어 있습니다.

```dart
TableCalendar(
  // .. 필수 값 3가지 작성
  headerStyle: HeaderStyle(
    this.titleCentered = false,  // 헤더 텍스트 가운데 정렬 여부
    this.formatButtonVisible = true,  // Format 버튼 보임 여부
    this.formatButtonShowsNext = true,  // Format 버튼 내부 텍스트 제어 여부
    this.titleTextFormatter,
    this.titleTextStyle = const TextStyle(fontSize: 17.0),  // 헤더 텍스트 스타일링
    this.formatButtonTextStyle = const TextStyle(fontSize: 14.0),  // Format 버튼 내부 텍스트 스타일링
    this.formatButtonDecoration = const BoxDecoration(  // Format 버튼 스타일링
      border: const Border.fromBorderSide(BorderSide()),
      borderRadius: const BorderRadius.all(Radius.circular(12.0)),
    ),
    this.headerMargin = const EdgeInsets.all(0.0),
    this.headerPadding = const EdgeInsets.symmetric(vertical: 8.0),
    this.formatButtonPadding =
        const EdgeInsets.symmetric(horizontal: 10.0, vertical: 4.0),
    this.leftChevronPadding = const EdgeInsets.all(12.0),
    this.rightChevronPadding = const EdgeInsets.all(12.0),
    this.leftChevronMargin = const EdgeInsets.symmetric(horizontal: 8.0),
    this.rightChevronMargin = const EdgeInsets.symmetric(horizontal: 8.0),
    this.leftChevronIcon = const Icon(Icons.chevron_left),
    this.rightChevronIcon = const Icon(Icons.chevron_right),
    this.leftChevronVisible = true,
    this.rightChevronVisible = true,
    this.decoration = const BoxDecoration(),
  )
)
```

<br />

## 캘린더 전체적인 스타일링 (calendarStyle: CalendarStyle())

켈린더의 헤더 영역을 제외한 나머지 영역을 스타일링 하는 방법입니다.

`calendarStyle`은 `headerStyle`과 달리 너무 많은 값들이 있기 때문에 자주 사용되는 몇가지만 작성하겠습니다.

```dart
TodayCalendar(
  calendarStyle: CalendarStyle(
    isTodayHighlighted: false,  // 오늘 날짜 하이라이트 보임 여부
    defaultDecoration: BoxDecoration(  // 주말과 선택된 날짜를 제외한 나머지 날짜에 대한 박스 스타일링
      color: Colors.grey,
    ),
    weekendDecoration: BoxDecoration(),  // 주말에만 박스 스타일링 (선택한 날짜는 적용 X)
    selectedDecoration: BoxDecoration(),  // 선택된 날짜에만 박스 스타일링
    
    defaultTextStyle: TextStyle(),  // 주말과 선택된 날짜를 제외한 나머지 날짜에 텍스트 스타일링
    weekendTextStyle: TextStyle(),  // 주말에만 텍스트 스타일링 (선택된 날짜는 적용 X)
    selectedTextStyle: TextStyle(),  // 선택된 날짜에만 텍스트 스타일링
  )
)
```

> 위의 값들 말고도 다양한 값들이 있습니다.

<br />

## 특정 날짜 선택 시 이벤트 (onDaySelected)

TableCalendar 위젯에서 날짜를 선택할 경우 이벤트를 발생시킬 수 있습니다.

``` dart
DateTime? selectedDay;

TableCalendar(
  // selectedDay - 사용자가 선택한 날짜
  // focusedDay - 사용자가 선택하여 포커싱된 날짜
  onDaySelected: (DateTime selectedDay, DateTime focusedDay) {
    setState(() {
      this.selectedDay = selectedDay;
    });
  },
  
  // 지정된 날짜를 선택된 날짜로 표시할지 여부를 결정하는 함수 (bool 타입 반환)
  selectedDayPredicate: (DateTime day) {
    if (selectedDay == null) {
      return false;
    }
    
    return day.year == selectedDay!.year && day.month == selectedDay!.month && day.day == selectedDay!.day;   
  }
)
```

> 위와 같이 작성하면 캘린더에서 사용자가 날짜를 선택하면 포커싱이 바뀌는 것을 확인할 수 있습니다.ㄴ

