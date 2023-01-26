 

# ImagePicker

이미지 라이브러리에서 이미지를 선택하고 카메라로 새 사진을 찍을 수 있는 iOS 및 Android용 Flutter 플러그인입니다.

<br />

## 플러그인 추가

[pub.dev](https://pub.dev/packages/image_picker)에 접속하여 ImagePicker 라이브러러 복사 후 `pubspec.yaml` 파일에 추가해준 뒤 `pub get`을 해줍니다.

<br />

## 설정

IOS 같은 경우 약간의 설정이 필요한데, 권한 허용 유무에 대한 Confirm 창에 들어갈 문구를 설정해줘야 합니다.

`IOS > Runner > info.plist` 파일을 엽니다.

info.plist 파일 하위에 아래와 같이 작성

```
	<key>NSPhotoLibraryUsageDescription</key>
	<string>사진첩 권한을 허가해주세요.</string>
	<key>NSCameraUsageDescription</key>
	<string>카메라 권한을 허가해주세요.</string>
	<key>NSMicrophoneUsageDescription</key>
	<string>마이크 권한을 허가해주세요.</string>
</dict>
</plist>
```

<br />

## 이미지 / 동영상 선택

``` dart
import 'package:image_picker/image_picker.dart';

void exampleImagePicker() async {
  // 이미지 1장
  final image = await ImagePicker().pickImage(source: ImageSource.camera);


  // 이미지 여러 장
  final multiImage = await ImagePicker().pickMultiImage(
    maxWidth: 30.0,  // 이미지 최대 너비 (설정하지 않으면 원본 이미지 너비 반환)
    maxHeight: 30.0,  // 이미지 최대 높이 (설정하지 않으면 원본 이미지 높이 반환)
    imageQuality: 3,  // 0 ~ 100 까지 이미지 품질 설정
    requestFullMetadata: true, // 기본 값은 true 이며 플러그인이 가져올 추가 정보의 양을 제어
  );
  
  // 동영상 카메라로 촬영하여 선택
  final video = await ImagePicker().pickVideo(source: ImageSource.camera);
}
```

> `Imagesoucre.camera` : 카메라로 촬영하여 선택
>
> `Imagesoucre.gallery` : 앨범(갤러리)에서 선택
>
> Video는 1장만 선택 가능

<br />

## 선택한 이미지 / 동영상 화면에 렌더링

위에 있는 image, video의 변수를 생성자에 할당 받는 방식으로 구현

``` dart
import 'dart:io';


class ImagePickerScreen extends StatefulWidget {
  final XFile image;  // 이미지
  final XFile video;  // 동영상
  
  const ImagePickerScreen({
    required this.image,
    required this.video,
    Key? key,}) : super(key: key);

  @override
  _ImagePickerScreenState createState() => _ImagePickerScreenState();
}

class _ImagePickerScreenState extends State<ImagePickerScreen> {
  VideoPlayController? videoController;
  Image? imageController;
  
  @override
  void initState() {
    super.initState();
    initializeController();
  }
  
  initializeController() async {
    // 동영상
    videoController = VideoPlayerController.file(
      File(widget.video.path),
    );
    
    await videoController!.initialize();
    
    // 이미지
    imageController = Image.file(
      File(widget.image.path),
    );
    
    // initializeController 함수가 실행 될 때 빌드를 다시 하기 위해 선언
    setState(() {});
  }
  
  @override
  Widget build(BuildContext context) {
    // 동영상 렌더링 할 경우 (이미지 렌더링 할 경우 videoController -> imageController)
    // null일 경우 로딩 화면 보여지도록 설정
    if (videoController == null) {
      return CircularProgressIndicator(
        color: Colors.black,
      );
    }
    
    // 동영상 렌더링
    return AspectRatio(
      aspectRatio: videoController!.value.aspectRatio,
      child: VideoPlayer(videoController!),
    );
    
    // 이미지 렌더링
    return imageController;
  }
}
```

