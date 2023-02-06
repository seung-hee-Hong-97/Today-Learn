# Flutter Secure Storage

`Android`에서는 `keystore` 영역에, `ios` 에서는 `keychain`라는 내부 저장소 영역을 사용합니다.

Android의 keystore 영역은 소스코드 내부 어딘가가 아닌 `시스템 만이 접근할 수 있는 컨테어너`에 저장하기 때문에 루팅을 하여도 접근할 수 없습니다.

그렇기 때문에 앱에서 임의로 발급한 암호화 키만 저장하고 이 키를 이용하여 정보를 암호화 하여 로컬 DB에 저장하고, 저장된 정보를 사용할 때는 복호화 하여 사용하는 라이브러리 입니다.

<br />

## 의존성 추가

[pub.dev](https://pub.dev/packages/flutter_secure_storage)에 접속하여 flutter_secure_storage를 pubspec.yaml 파일에 의존성 추가한 뒤 pub get을 해줍니다.

또한 Android 같은 경우 minSdkVersion을 맞춰줘야 합니다.

해당 문서에 나와 있는 버전으로 설정하면 됩니다.

`android / app / build.gradle`

```xml
android {
    ...
    defaultConfig {
        ...
        minSdkVersion ^version
        ...
    }
}
```

<br />

## Storage 생성

``` dart
final storage = FlutterSecureStorage();
```

<br />

## 값 불러오기

``` dart
String value = await storage.read(key: key);
```

<br />

## 모든 값 불러오기

``` dart
Map<String, String> allValues = await storage.readAll();
```

<br />

## 값 삭제

``` dart
await storage.delete(key: key);
```

<br />

## 모든 값 삭제

``` dart
await storage.deleteAll();
```

<br />

## 값 저장하기

``` dart
await storage.write(key: key, value: value);
```

