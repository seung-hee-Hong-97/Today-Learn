# GeoLocator 

사용중인 기기의 위치정보를 가져오는 기능을 지원한다.  

추가적으로 위/경도를 사용한 좌표계 상 직선거리의 계산을 보조해주기도 한다.

기기의 위치를 Stream을 통해 지속적으로 받아오거나 

<br />

## Geolocator.getPositionStream()

기기의 위치를 **지속적으로** 받아옵니다.

위치가 변경됨에 따라 변경된 위치의 값을 지속적으로 가져올 수 있으며, 메서드명에서 볼 수 있듯이 stream 형태로 값을 반환합니다.

```dart
Geolocator.getPositionStream();
```

<br />

## Geolocator.getCurrentPosition() 

기기의 위치를 **한번** 받아옵니다. 

호출 시점의 위치를 반환하며 Future 형태로 값을 반환합니다.

```dart
Geolocator.getCurrentPosition();
```

<br />

## Geolocator.isLocationServiceEnabled()

위치 서비스 버튼 활성화 여부를 확인합니다.

Bool 타입의 값을 반환합니다.

```dart
Geolocator.isLocationServiceEnabled();
```

<br />

## Geolocator.checkPermission()

현재 앱이 가지고 있는 권한에 대해 확인합니다. -> 권한을 허용 했는지, 안했는지

LocationPermission 타입으로 반환 합니다.

``` dart
LocationPermission checkPermission = Geolocator.checkPermission();
```

<br />

### LocationPermission

- `denied` : 기본 값이며 일반적인 권한 요청을 수행
- `deniedForever` : 권한 요청을 했을 때 사용자가 권한을 안준 상태 -> 개발자가 이 앱에서 권한 허가를 받을 수 없음 -> 권한 허가를 하기 위해선 사용자가 직접 설정에 들어가서 권한 허용을 해줘야 함
- `whileInUse` : 앱을 실행 중에만 권한 허용
- `always` : 항상 허용

