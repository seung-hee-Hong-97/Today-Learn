# Modifier

Provider 뒤에 `.`을 붙혀서 사용하는 형태이고, 모든 Provider에 사용할 수 있습니다.

<br />

## .family

`.family`는 Provider 뒤에 사용합니다. 외부 값을 이용하여 Provider를 생성할 때 사용합니다.

``` dart
// .family modifier를 사용하면 Generic 타입에 외부에서 받아올 값의 타입을 넣어줘야 함
// 파라미터에 외부에서 받아올 값을 추가해줘야 함 -> data
final familyModifierProvider = FutureProvider.family<List<int>, int>((ref, data) async {
  await Future.delayed(Duration(seconds: 2));
  
  return List.generate(5, (index) => index * data);
})
```

```dart
class FamilyModifierScreen extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final state = ref.watch(familyModifierProvider(3));  // .family modifier에 3이라는 값을 파라미터에 넣음
    
    return // 위젯;
  }
}
```

<br />

## .autoDispose

데이터 캐싱이 된 것을 자동으로 삭제하는 Modifier

필요 없을 때 삭제하고 필요할 때 다시 생성함

``` dart
final autoDisposeModifierProvider = FutureProvider.autoDispose<List<int>>((ref) async {
  await Future.delayed(Duration(seconds: 2));
  
  return [1, 2, 3, 4, 5];
})
```

> `autoDisposeModifierProvider`를 통해 데이터를 렌더링하는 페이지는 접속할 때 마다 데이터를 불러오는 것을 확인할 수 있음
>
> 왜냐하면 페이지를 벗어나면 캐싱을 자동으로 삭제하기 때문
