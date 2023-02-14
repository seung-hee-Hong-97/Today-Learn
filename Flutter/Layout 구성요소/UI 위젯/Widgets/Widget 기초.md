# Widget 





## MaterialApp / CupertinoApp

각각 Android / iOS의 대표 테마를 의미한다. Appbar 내부 텍스트의 정렬, 기본 아이콘 등의 테마들을 정의한다. 

위젯들의 기본 테마들을 지정한다. 위젯 내부에서 특정 스타일을 사용하도록 명시하지 않으면 해당 디자인 철학을 따른다.

아이콘의 모양도 바뀐다.



## Scafold 

화면 구성을 더 쉽게 하기 위한 대략적인 구분을 제공한다. 노치, UI에 따른 예외처리 등이 적용되기도 한다. 

### appbar

최상단의 bar를 의미한다.

Appbar() 위젯을 통해 내부의 요소들을 구현한다.

### body

중단의 일반적인 contents들이 위치한다.

### footer

하단에 위치하며 하단 navigation bar 와 같은 UI를 구성할때 스인다.



# Const Constructor

const 는 컴파일 시점에 정의되며 변경되지 않는다는 의미이다. 

const로 선언한 위젯은 stateful에 속해있어도 다시 랜더링 되지 않는다.



## Widget 조건문 활용 

child Widget호출 시 상위에 if문을 명시할 경우 if문에 결과에 따라 해당 위젯의 랜더링이 이루어진다.

```dart
Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          ...,
          if(isShowButton) // 조건문
          TextButton(onPressed: onPressed, child: Text("TEXT"))
        ],
      )
    ...
```

