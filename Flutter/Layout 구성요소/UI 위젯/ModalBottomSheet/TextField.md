# TextField

사용자가 숫자, 텍스트 등의 입력을 할 수 있는 영역을 만들어주는 위젯

<br />

``` dart
TextField(
  controller: controller,  // TextField에 표시되는 텍스트 제어
  onSubmitted: onSubmitted,  // TextField에 입력 완료 후 이벤트 발생 시 onSubmitted 콜백 함수 호출
  onChanged: onChanged,  // TextField의 텍스트가 변경 될 때 마다 onChanged 콜백 함수 호출
  expands: bool 타입 값,  // 차지할 수 있는 영역을 TextFiled가 전부 차지할 지에 대한 여부
  maxLines: int 타입 값,  // TextField의 최대 줄 (ex -> 3이면 3줄이 최대)
  keyboardType: TextInputType.키보드 타입 값,  // 숫자, 텍스트
  inputFormatters: [FilteringTextInputFormatter.digitsOnly],  // 입력 포맷 설정
  cursorColor: Colors.색상,  // 커서 색상
  decoration: InputDecoration(
    border: InputBorder.값,  // TextField 경계선 색
    filled: bool 타입 값,  // 배경색 설정 여부
    fillColor: Colors.색상,  // 배경색
    hintText: 'dfd',  // 웹의 placeholder와 동일 (도움말 -> 입력 시 사라짐)
  ),
)
```

<br />

## TextFormField 위젯

- `TextField` 위젯과 거의 동일하고 몇가지만 변형된 위젯입니다.

- TextFormFiield 위젯을 사용하는 이유는 사용자 입력창이 많을 때 TextField 위젯을 사용할 경우 입력창의 값을 관리하기 비효율적이기 때문입니다.

- TextFormField 위젯을 사용하면 여러 개의 텍스트를 관리할 수 있습니다. (validator, onSaved 등)
- TextFormField 위젯은 Form 위젯으로 감싼 후에 통상적으로 사용합니다.

``` dart
TextFormField(
  // null이 리턴되면 에러가 없음
  // 에러가 있을 경우 String 값으로 리턴
  validator: (String? val) {
    // ...
  }
  
  // TextFormField 위젯을 감싸고 있는 상위 위젯인 Form 위젯에서 save 함수를 실행 했을 때 실행됨
  onSaved: (String? val) {
    // ...
  }
)
```

<br />

## Form 위젯

- `child` 파라미터와 `key` 파라미터를 받는 위젯입니다.
- `child` : TextFormField 위젯을 넣어줍니다.
- `key` : GlobalKey를 넣어줍니다.

Key는 Form 내부의 TextFormField 값들을 저장하고 validation(유효성 체크)를 하는데 사용합니다.

``` dart
final GlobalKey<FormState> formKey = GlobalKey();

Form(
  key: formKey,
  autovalidateMode: AutovalidateMode.always, // 항상 자동으로 유효성 체크
)
```

<br />

### GlobalKey

제네릭 타입이 `FormState`인 케이스만 다뤄보도록 하겠습니다.

``` dart
// TextFormField 위젯의 validator 함수에서 반환해주는 bool 값
// 유효성 체크 후 조건에 충족하면 true, 아니면 false
formKey.currentState!.validate()
  
// 해당 함수가 실행되면 TextField 위젯의 onSaved 함수가 실행
formKey.currentState!.save()
```

