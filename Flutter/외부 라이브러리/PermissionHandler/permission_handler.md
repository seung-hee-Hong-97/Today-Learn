# permission_handler

특정 기능을 사용하는데 있어서 권한 요청을 하고 상태를 확인할 수 있는 API를 제공하고, 사용자가 권한을 부여할 수 있도록 기기의 앱 설정을 열도록 해줍니다.

<br />

## Permission

확인 및 요청할 수 있는 권한을 정의합니다.

<br />

### 다양한 기능들에 대한 권한 요청 및 확인 예시

``` dart
Permission.camera  // 카메라 기능에 대한 권한 요청 및 확인
Permission.microphone // 마이크 기능에 대한 권한 요청 및 확인
Permission.calendar  // 달력 기능에 대한 권한 요청 및 확인


Future<bool> init() async {
  // 카메라, 마이크, 달력 기능에 대해 권한 요청
  final response = await [Permission.camera, Permission.microphone, Permission.calendar].request();
  
  // 각각 권한 요청에 대한 응답을 받음
  final cameraPermission = response[Permission.camera];
  final micPermission = response[Permission.microphone];
  final calendarPermission = response[Permission.calendar];
}
```

> 예시로 든 3가지 말고도 많은 기능에 대한 권한 요청이 있습니다.

<br />

## PermissionStatus

권한에 대한 상태를 정의합니다.

<br />

### denied

아직 권한 요청을 하지 않은 상태 (권한을 물어보지 않은 상태)

``` dart
PermissionStatus.denied
```

<br />

### granted

권한 요청이 허용된 상태

``` dart
PermissionStatus.granted
```

<br />

### permanentlyDenied

권한 요청이 거부 당한 상태 -> 거부 당하면 권한을 재 요청 할 수 없음 -> 사용자가 직접 권한 설정을 해줘야 함

``` dart
PermissionStatus.permanentlyDenied
```

<br />

### restricted

IOS에서만 지원되는 기능이며, 보호자가 임의적으로 권한 설정을 했을 때 부분 권한 설정이 된 상태

``` dart
PermissionStatus.restricted
```

<br />

### limited

IOS에서만 지원되는 기능임여, 사용자가 직접 몇가지 권한만 허용한 상태

``` dart
PermissionStatus.limited
```

<br />

## 권한 요청 사용 예시

``` dart
FutureBuilder<bool>(
  future: init(),
  builder: (_, snapshot) {
    // 권한이 없을 경우 실행
    if (snapshot.hasError) {
      return Center(
        child: Text(snapshot.error.toString()),
      );
    }
    
    // 권한 요청에 대한 데이터가 없을 경우 로딩창 렌더링
    if (!snapshot.hasData) {
      return Center(
        child: CircularProgressIndicator(),
      );
    }
    
    // ...
  }
)



Future<bool> init() async {
  final response = await [Permission.camera, Permission.microphone].request();

  final cameraPermission = response[Permission.camera];
  final micPermission = response[Permission.microphone];

  if (cameraPermission != PermissionStatus.granted || micPermission != PermissionStatus.granted) {
    throw '카메라 또는 마이크 권한이 없습니다.';
  }
  
  return true;
}
```