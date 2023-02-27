
# OOP 일반화 

코드의 중복을 줄이고 프로세스의 직렬화를 위해 상속(extends)과 다형성(interface)을 통해 코드를 작성한다. 

경우에 따라 제네릭을 사용하기도 한다. 



## 상속



### 상속 클래스의 구현

```dart
abstract class CursorPaginationBase {}

class CursorPaginationError extends CursorPaginationBase {
  final String message;
  CursorPaginationError({required this.message});
}

class CursorPaginationLoading extends CursorPaginationBase {
  CursorPaginationLoading();
}

@JsonSerializable(genericArgumentFactories: true)
class CursorPagination<T> extends CursorPaginationBase {
  final CursorPaginationMeta meta;
  final List<T> data;

  CursorPagination({required this.meta, required this.data});

  factory CursorPagination.fromJson(
          Map<String, dynamic> json, T Function(Object? json) fromJsonT) =>
      _$CursorPaginationFromJson(json, fromJsonT);
  CursorPagination copyWith({CursorPaginationMeta? meta, List<T>? data}) {
    return CursorPagination(meta: meta ?? this.meta, data: data ?? this.data);
  }
}

// 새로고침
class CursorPaginationRefetching<T> extends CursorPagination<T> {
  CursorPaginationRefetching({required super.data, required super.meta});
}

// 기존 데이터에 추가로 데이터를 가져오는 경우
// loading의 경우 데이터가 없음을 가정하기 때문에
class CursorPaginationRefetchingMore<T> extends CursorPagination<T> {
  CursorPaginationRefetchingMore({required super.data, required super.meta});
}

```



위의 코드는 Paging을 통해 데이터를 받아오는 동작의 이벤트들을 정의한 코드이다. 

모든 클래스는 ```CursorPaginationBase``` 을 싱속받으며 아래의 새로고침과 데이터 추가 요청은 ```CursorPagination```을 상속받는다



### 클래스 상속의 사용 예시

```dart
class PaginationProvider<T extends IModelWithId,
        U extends IBasePaginationRepository<T>>
    extends StateNotifier<CursorPaginationBase> {
  PaginationProvider({required this.repository})
      : super(CursorPaginationLoading());

  final U repository;

  Future<void> paginate(
      {int fetchCount = 20,
      // true - 값 추가 false - 값 새로고침
      bool fetchMore = false,
      // 강제로 다시 로딩하기. <- true인 경우 실행
      bool forceRefech = false}) async {
    /// state의 5가지 상태
    /// 1. cursorpagination - 정상적으로 데이터가 있음
    /// 2. cursorpaginationLoading - 데이터가 로딩 중인 상태(캐시 없음)
    /// 3. cursorpaginationError - 오류
    /// 4. cursorpaginationRefeching 첫 페이지부터 값 가져오기
    /// 5. cursorpaginationFetchMore 추가 데이터를 가져오는 요청

    /// 바로 값을 반환해야 하는 경우
    /// 1. hasMore - false(기존 상태에서 이미 다음 데이터가 없다는 값을 가진다면)
    /// 2. loading - fetchMore가 true 인 경우
    ///  - loading 중이나 fetchMore가 fasle 인 경우
    ///  <- 데이터를 가져오는 중 기존 데이터를 새로고침 한다.
    /// 기존 데이터를 다시 가져오는 것을 의도함

    try {
      if (state is CursorPagination && !forceRefech) {
        final pState = state as CursorPagination;
        // 더 가져올 수 있는 값이 있는지 확인한다.
        if (!pState.meta.hasMore) {
          return;
        }
      }

      final isLoading = state is CursorPaginationLoading;
      final isRefeching = state is CursorPaginationRefetching;
      final isRefechingMore = state is CursorPaginationRefetchingMore;

      if (fetchMore && (isLoading || isRefeching || isRefechingMore)) {
        return;
      }

      PaginationParams paginationParams = PaginationParams(
        count: fetchCount,
      );

      // 데이터 추가 요청 케이스 <- 이미 있는 데이터에 값을 추가 함
      if (fetchMore) {
        final pState = state as CursorPagination<T>;

        state = CursorPaginationRefetchingMore<T>(
            data: pState.data, meta: pState.meta);

        paginationParams = paginationParams.copyWith(
          after: pState.data.last.id,
        );
      } else {
        // 기존 데이터가 있다면 기존 데이터를 보존하고 fetch
        if (state is CursorPagination && !forceRefech) {
          final pState = state as CursorPagination<T>;
          state = CursorPaginationRefetching<T>(
              data: pState.data, meta: pState.meta);
        } else {
          // 데이터를 모두 없에고 로딩
          state = CursorPaginationLoading();
        }
      }

      final resp = await repository.paginate(params: paginationParams);
      if (state is CursorPaginationRefetchingMore<T>) {
        final pState = state as CursorPaginationRefetchingMore<T>;

        // 기존 데이터에 새로운 데이터 추가
        state = resp.copyWith(data: [...pState.data, ...resp.data]);
      }
      // 기존 데이터를 계속 보여주는 경우
      else {
        state = resp;
      }
    } on Exception catch (e) {
      state = CursorPaginationError(message: e.toString());
    }
  }
}

```

상속된 값들을 실제로 사용하는 코드이다. 

해당 함수에서 발생하는 이벤트는 다음과 같다. 

1. 데이터 로딩 `CursorPaginationLoading`
2. 오류발생 `CursorPaginationError`
3. 데이터요청 `CursorPagination`
4. 새로고침 `CursorPaginationRefetching`
5. 데이터 추가요청 `CursorPaginationRefetchingMore`



## 1. 요청인자의 최소화

해당 클래슨는 생성자에서 `CursorPaginationBase` 클래스를 인자로 받도록 명시되어 있다.  

이후내부 로직을 처리하는 과정에서 해당 클래스를 상속받는 클래스들을 사용하여 로직을 구현한다.



## 2. 로직 변경에 따른 코드 변경범위의 하락

운영단계에서 추가적인 요청사항으로 인해 로직을 추가하거나 조건이 추가되어도 해당 클래스의 호출에는 영향을 주지 않는다.



 ## 3. 코드 해석 단계에서 가독성 

2017년 발표된 논문에 따르면 개발자의 업무시간 중 60% 이상은 기존 코드를 보고 이해하는 것에 사용한다고 한다. 

https://ieeexplore.ieee.org/abstract/document/7997917

해당 프로세스의 흐름을 이해하기 위해 해석해야 하는 코드의 단위는 작을수록 좋다. 

