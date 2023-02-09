# Consumer

Consumer로 감싼 위젯에서 일부분만 재 빌드 되도록 설정할 수 있는 Riverpod 라이브러리 위젯

<br />

## 사용 방법

``` dart
Consumer(
  builder: (BuildContext context, WidgetRef ref, Widget? child) {
    return // 위젯;
  }
  child: //위젯
)
```

> `child` : child 파라미터에 넣은 위젯은 builder 함수가 재 실행이 되어도 처음 한번만 빌드가 되고 그 뒤로는 빌드가 되지 않음
>
> `builder` : 해당 함수에서 리턴하는 위젯의 상태가 변경되어도 다른 위젯들은 영향을 받지 않고 builder 함수 내에 있는 위젯만 재 빌드 되기 때문에 조금의 성능 향상을 볼 수 있음