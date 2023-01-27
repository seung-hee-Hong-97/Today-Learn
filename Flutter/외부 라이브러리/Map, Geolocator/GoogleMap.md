# Google Map

<br />

## google_maps_flutter

```dart
import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';

class HomeScreen extends StatefulWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  _HomeScreenState createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  // 위도, 경도 설정
  static final LatLng myLatLng = latLng(
    37.5233273,
    126.921252,
  );
  
  // 지도 렌더링 시 최초 카메라 위치 (지도의 중앙에 위치할 위도 / 경도 설정)
  static final CameraPosition initialPosition = CameraPosition(
    target: myLatLng,
    zoom: 15,  // 지도 확대 
  );
  
  // Circle 설정
  static final Circle circle = Circle(
    circleId: CircleId('circle'),  // 생성하는 원의 고유 값 (필수)
    center: myLatLng,  // 원의 위치 (위도 / 경도)
    fillColor: Colors.blue.withOpacity(0.5);  // 원의 배경색
    radius: 100,  // 원의 범위 (미터 단위)
    strokeColor: Colors.blue,  // 원의 경계선 색
    strokeWidth: 1,  // 원의 경계선 굵기
  );
  
  // Marker 설정
  static final Marker marker = Marker(
    markerId: MarkerId('marker'); // 생성하는 마커의 고유 값 (필수)
    position: myLatLng, // 마커의 위치 (위도 / 경도)
  );
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: GoogleMap(
        initialCameraPosition: initialPosition,
        myLocationEnabled: true,
        myLocationButtonEnabled: false,
        circle: Set.from([circle]),
        markers: Set.from([marker]),
      ),
    );
  }
}

// Circle, Marker는 Set으로 설정하기 때문에 ID가 동일하면 여러 개를 적용해도 1개만 지도에 보입니다.
```

> ### GoogleMap 클래스의 속성
>
> - `mapType` : 지도의 종류 (일반, 위성 등등)
> - `initialCameraPosition` : 최초 카메라 위치 (지도 중앙 부분에 위치할 위도 경도 설정)
> - `myLocationEnabled` : 현재 나의 위치를 나타내는 것의 허용 여부 (true / false)
> - `myLocationButtonEnabled` : 지도를 현재 나의 위치로 이동하도록 하는 버튼 보임 여부 (true / false)
> - `circle` : 마커 주변에 원 모양이 나오도록 설정 (Set을 통해 설정)
> - `markers` : 특정 위치에 마커 나오도록 설정 (Set을 통해 설정)
> - `onMapCreated` : MapController를 통해서 지도에 특정 이벤트를 설정
