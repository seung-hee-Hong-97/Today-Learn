# App에서 Capacitor 3.0으로 업데이트

Ionic Framework를 통해 프로젝트를 하는 개발자들은 필요에 따라 Plugin을 사용한다.

예전엔 Cordova-Plugin을 위주로 사용을 하다가, 점점 Capacitor Plugin의 사용 빈도수가 늘어났는데, `2021.03.10`에 Capacitor 3.0 Beta가 출시되었고, 얼마 지나지 않아 정식버전이 출시되었다.

따라서 기존에 Capacitor 2.0 버전을 사용을 했다면, 업데이트를 해줘야 하기 때문에 아래의 과정을 잘 따라오면 된다.

업데이트 과정은 Ionic Framework를 사용하고, Capacitor를 기반으로 한다는 전제하에 진행한다. 만일 Ionic Framework를 설치해야한다면, [여기](https://github.com/sejong77/Today-Learn/blob/Master/Ionic%20FrameWork/Ionic%20CLI%20%EC%84%A4%EC%B9%98%20%EB%B0%8F%20%EC%8B%A4%ED%96%89%20%EB%A7%A4%EB%89%B4%EC%96%BC.md) 를 참조하면 된다.



## Capacitor 3.0을 사용하기 위한 조건들

우선 Capacitor 3.0 버전을 사용하기 위해선 몇 가지 조건들이 필요하다.



### NodeJs 12 +

Capacitor 3.0 버전을 사용하려면 NodeJs 12 이상이 갖춰져야 한다.

본인의 NodeJs 버전을 확인한 뒤, 버전이 12보다 낮다면, 최신 버전을 설치해준다.

```bash
# NodeJs 버전 확인
$ node --version
```

> 만약 버전이 12보다 낮은 버전이라면, [여기](https://nodejs.org/ko/download/)에 들어가서 최신 버전을 다운로드 해준다.



### TypeScript 3.8 +

TypeScript도 버전이 3.8이상이 되야 하는데, 본인의 TypeScript 버전을 확인한 뒤, 버전이 3.8보다 낮다면, 최신 버전을 설치해준다.  버전을 확인하는 방법은 2가지가 있는데, 터미널을 사용하거나, `package.json` 파일을 확인하는 것이다.

#### 터미널로 확인

```bash
# TypeScript 버전 확인
$ ng --version
```

> 위의 명령어를 입력하면, TypeScript의 버전뿐만 아니라, Angular의 버전, rxjs의 버전 등 다양한 버전이 나오니 참고하면 좋다.
>
>  단 Angular가 설치되어 있어야, ng 명령어를 사용할 수 있으니 Angular가 설치되어 있지 않다면, 설치를 해야줘야 한다.
>
> [Angular 설치하기](https://angular.io/guide/setup-local)



#### package.json 파일로 확인

본인이 생성한 프로젝트가 있다면, `package.json` 파일이 있을 것인데, 아래의 이미지처럼 여러 버전들을 확인할 수 있다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/package.json%20%EC%9D%B4%EB%AF%B8%EC%A7%80.png?raw=true">



### ES2017 +

Capacitor 3.0은 ES5 대신에 ES2017 이상 버전을 사용해야 한다.

본인의 버전을 확인하는 방법은 `tsconfig.json` 파일을 확인하면 된다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/tsconfig.json%20%EC%9D%B4%EB%AF%B8%EC%A7%80.png?raw=true">

> 여기서 module, lib 부분의 버전을 확인하여 ES2017보다 낮은 버전을 사용하고 있다면, ES2017보다 높은 버전으로 수정해준다.



위의 조건사항들에 모두 충족이 되었다면, 본격적으로 Capacitor 3.0 버전으로 업데이트를 시작할 수 있다.

먼저 터미널에 아래의 명령어를 입력하여 가장 최신 버전의 `Capacitor/core` 와 `Capacitor/cli` 로 업데이트 해준다.

```bash
$ npm install @capacitor/cli@latest @capacitor/core@latest
```

> 이미 Capacitor를 사용하고 있었다면, 가장 최신버전인 3.0으로 업데이트가 될 것이고, 설치되어 있지 않다면, 바로 3.0 버전으로 설치가 될 것이다.



## Plugin 사용법 변경

### 기존에 Plugin 사용하던 방식

기존에 사용하던 Capacitor 2.0 버전에서는 Plugin을 사용할 때, 아래의 코드와 같이 Cordova-Plugin처럼 사용하려는 Plugin을 설치하지 않고, import를 시켜주고 사용을 하면 됐었다.

``` typescript
// Capacitor 2.0 버전에서 Plugin 사용하던 방식

import { Plugins } from '@capacitor/core';

const { Clipboard } = Plugins;
```

> 따로 Plugin을 설치하지 않고, `Plugins` 를 import 시켜준 후, 사용하려는 Plugin에 `Plugins` 를 할당하는 방식



### 업데이트 후 Plugin 사용하는 방식

Capacitor 3.0 버전에서는 Plugin을 개별적으로 설치한 후에 사용할 수 있다.

예를 들어, `Storage` 라는 Capacitor Plugin을 사용하고 싶다면, 먼저 터미널에서 아래의 명령어를 입력해 설치를 해준다.

```bash
$ npm install @capacitor/storage

$ npx cap sync
```

> npx cap sync를 꼭 입력해줘야 설치한 파일들이 동기화된다.



성공적으로 설치가 되었다면, 아래의 코드를 입력하면 정상적으로 Capacitor 3.0 버전의 Plugin을 사용할 수 있다.

```typescript
import { Storage } from '@capacitor/storage';
```



지금까지의 과정을 잘 따라왔다면, Capacitor 3.0 버전으로 업데이트가 진행됐을 것이고, Plugin을 사용하는 방법도 알게 되었다. 이제부터는 `IOS`, `Android` 에서도 정상적으로 Capacitor 3.0 버전을 사용할 수 있도록 알아볼 것이다.



## IOS

우선 IOS에서 Capacitor 3.0 버전을 사용하려면 조건이 필요한데, `IOS 12+`   `Xcode 12+`  `CocoPods 1.8+` 의 버전이 필요하다.



### CocoPods 업데이트

Capacitor 3.0 버전을 사용하기 위해선, CocoPods의 버전도 1.8이상이 권장되는데, 우선 본인의 CocoPods의 버전을 확인하고 싶다면 아래의 명령어를 터미널에 입력하여, 버전을 확인해본다.

```bash
$ pod --version
```

> 만약 1.8이상의 버전이 설치되어 있다면 굳이 업데이트를 진행할 필요는 없지만, 1.8보다 낮은 버전 혹은 설치되어 있지 않다면 업데이트 및 설치를 진행해줘야 한다.
>
> [여기](https://cocoapods.org/) 를 참조하여 진행하면 수월할 것이다.



### IOS 배포대상을 12.0으로 설정

우선 Xcode에 접속을 한다.

진행하고 있는 프로젝트에 들어가고, 만약 진행하고 있는 프로젝트가 없다면 새롭게 생성을 해준다.

`IOS > App > Build Settings > Deployment > IOS Deployment Target` 에 있는 버전을 `12.0` 이상으로 설정해준다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/Xcode%20BuildSetting.png?raw=true">



설정을 해줬다면, `IOS > Pods > Podfile `  의 경로로 들어가서, 아래와 같이 버전을 `12.0` 으로 바꿔준다.

```swift
-platform :ios, '11.0'
+platform :ios, '12.0'
 use_frameworks!
```



### Swift 버전을 5로 설정

현재 Xcode의 앱에서 Swift의 버전을 확인하고, `Swift 5` 가 아니라면, 변경 해준다.

`IOS > App > Build Setting > Swift Compiler - Language` 에 들어가서 변경을 해주면 된다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/Xcode%20Swift%20Version.png?raw=true">





### Public 폴더를 IOS 대상 디렉토리로 이동

아마 기존에는 `IOS/App/public` 형태의 구조로 되어 있을 것인데, Capacitor 3.0 버전부터는 `Ios/App/App/public` 형태의 구조로 변경을 해줘야 한다.

우선 기존의 `public` 폴더를 제거해줘야 하는데, 아래의 과정대로 진행하면 된다.

	1. `App` 프로젝트 아래의 파일트리를 확장해준 후에 `App` 그룹을 확장하고, `public` 폴더를 선택한다.
 	2. `public 폴더`에 마우스 오른쪽 버튼을 클릭한 후, `삭제(Delete)` 를 클릭하여, `public` 폴더를 삭제한다.

<img src="https://capacitorjs.com/assets/img/docs/ios/xcode-public-delete-folder.png">

 위의 과정대로 `public` 폴더를 삭제했다면, 새로운 위치에 재생성을 해줘야 한다. 이래의 과정을 따라서 진행하면 된다.

1. `IOS > App > App` 위치에 마우스 오른쪽 버튼을 클릭하여 `Add files to "App"...` 항목을 선택한다.
2. 그럼 새로운 창이 뜰 것인데, 옵션은 그대로 두고 새로 열린 창의 왼쪽 하단에 `New Folder` 를 클릭한다.
3. 폴더의 이름을 `public` 으로 입력하고 `create` 버튼을 클릭한 후에, `Add` 를 클릭해 최종적으로 생성해주면 완료

<img src="https://capacitorjs.com/assets/img/docs/ios/xcode-public-new-folder.png">

위의 과정을 통해 새로 생성한 `public` 폴더는 Xcode에서는 동일하게 보일 수 있지만 `App` 프로젝트가 루트가 아닌, App 그룹에 상대적인 폴더여야 한다.

이제 새로 생성한 `public` 폴더는 `gitignore` 파일에 추가시켜 줘야 한다. 추가시켜주는 이유는 `public` 폴더에 웹 자산의 사본이 포함되어 있으므로 커밋을 하면 안되기 때문이다.

`IOS/.gitignore` 파일을 수정해준다.

``` gitignore
 App/build
 App/Pods
-App/public
+App/App/public
 App/Podfile.lock
 xcuserdata
```

> 이렇게 설정을 해주면, 자동으로 `Àpp/App/public` 은 커밋되지 않는다.

 

### Capacitor IOS 플랫폼 업데이트

지금까지의 과정을 통해 IOS에서 Capacitor 3.0 버전을 사용할 수 있는 거의 모든 조건을 갖추었다고 보면된다.

이제 아래의 명령어를 터미널에 입력하여 Capacitor IOS 플랫폼을 최신 버전으로 업데이트 시켜준다.

```bash
$ npm install @capacitor/ios@latest

$ npx cap sync ios
```



### Application Event에서 `CAPBridge` ==> `ApplicationDelegateProxy` 로 전환

기존에 `Application Event` 에서 사용하던 `CAPBridge` 에서 `ApplicationDelegateProxy` 로 바꿔주는 작업을 해야한다.

`IOS/App/App/AppDelegate.swift` 에 있는 파일에 접속을 하고, 아래의 코드로 바꿔주면 된다.

``` swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    // Called when the app was launched with a url. Feel free to add additional processing here,
    // but if you want the App API to support tracking app url opens, make sure to keep this call
-		 return CAPBridge.handleOpenUrl(url, options)  
+    return ApplicationDelegateProxy.shared.application(app, open: url, options: options)
  }

func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool {
    // Called when the app was launched with an activity, including Universal Links.
    // Feel free to add additional processing here, but if you want the App API to support
    // tracking app url opens, make sure to keep this call
-		 return CAPBridge.handleContinueActivity(userActivity, restorationHandler)  
+    return ApplicationDelegateProxy.shared.application(application, continue: userActivity, restorationHandler: restorationHandler)
  }
```



### `CAPNotification` ==> `NSNotification` 확장으로 전환

이것도 위와 동일한 경로에 있는 파일에 접속을 하면 된다. `IOS/App/App/AppDelegate.swift`

``` swift
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
    super.touchesBegan(touches, with: event)

    let statusBarRect = UIApplication.shared.statusBarFrame
    guard let touchPoint = event?.allTouches?.first?.location(in: self.window) else { return }

    if statusBarRect.contains(touchPoint) {
     -	 NotificationCenter.default.post(CAPBridge.statusBarTappedNotification) 
     +   NotificationCenter.default.post(name: .capacitorStatusBarTapped, object: nil)
    }
  }

func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
-	  NotificationCenter.default.post(name: Notification.Name(CAPNotifications.DidRegisterForRemoteNotificationsWithDeviceToken.name()), object: deviceToken)
  
+    NotificationCenter.default.post(name: .capacitorDidRegisterForRemoteNotifications, object: deviceToken)
  }

func application(_ application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: Error) {
-  	 NotificationCenter.default.post(name: Notification.Name(CAPNotifications.DidFailToRegisterForRemoteNotificationsWithError.name()), object: error)

+    NotificationCenter.default.post(name: .capacitorDidFailToRegisterForRemoteNotifications, object: error)
  }
```



### `DerivedData` 를  .gitignore 파일에 추가

`IOS/.gitignore` 파일에 `DerivedData` 를 추가해준다. Capacitor CLI가 기본 IOS 빌드를 배치하는 곳이다.

```gitignore
 App/Pods
 App/App/public
 App/Podfile.lock
+DerivedData
 xcuserdata
```



여기까지가 Capacitor 3.0 버전을 IOS에서 사용할 수 있도록 업데이트 하는 과정이다.



## Android

Android도 Capacitor 3.0 버전을 사용하려면 몇 가지 조건이 필요하다.

	1. Android 5.0 이상 (현재는 Android 11 지원)
 	2. Android Studio 4.0 이상



우선 위의 조건을 확인하기 위해 `Android Studio > About Android Studio  ` 로 들어간다. 그러면 현재 사용 중인 Android Studio의 버전을 확인할 수 있는데, 만약 4.0보다 낮은 버전을 사용하고 있다면, 최신 버전으로 업데이트를 시켜준다. [업데이트 필요 시 참조](https://developer.android.com/studio/intro/update?hl=ko)



### Capacitor Android 플랫폼 업데이트

Capacitor 3.0 버전을 사용할 수 있는 조건이 갖춰졌다면 터미널에 아래의 명령어를 입력하여, 최신 버전의 Capacitor로 업데이트를 시켜준다.

```bash
$ npm install @capacitor/android@latest

$ npx cap sync android
```





### 자동으로 Android Plugin 로드

Capacitor 3.0 버전에서는 Android 플러그인을 자동으로 로드하는 것이 좋다.

우선 `MainActivity.java` 파일에 들어간 후, 아래의 코드와 같이, `onCreate()` 를 제거한다. 이제 `npm` 을 통해 설치된 플러그인을 추가하거나 제거할 때 더 이상 `MainActivity.java` 파일을 편집할 필요가 없다.

``` java
 public class MainActivity extends BridgeActivity {
-    @Override
-    public void onCreate(Bundle savedInstanceState) {
-        super.onCreate(savedInstanceState);
-
-        // Initializes the Bridge
-        this.init(savedInstanceState, new ArrayList<Class<? extends Plugin>>() {{
-            // Additional plugins you've installed go here
-            add(Plugin1.class);
-            add(Plugin2.class);
-        }});
-    }
 }
```



만약 앱에 특별히 Application 전용으로 빌드된 사용자 지정 플러그인이 포함되어 있는 경우에, `onCreate()` 에 아래의 코드처럼 입력을 하면 된다.

```java
 public class MainActivity extends BridgeActivity {
     @Override
     public void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);

+        registerPlugin(PluginInMyApp.class);
     }
 }
```





### Android Studio에서 Gradle을 7.0 버전으로 업데이트

Capacitor 3.0 버전은 `Gradle Version` 7.0 , `Android Gradle Plugin Version` 4.2.0으로 설정하는 것을 권장한다.

우선 Android Studio에 들어가서 다음의 과정을 따라 설정을 해주면 된다.

	1. `File > Project Structure ` 를 클릭하여 들어간다.
 	2. `Gradle Version`을 7.0으로, `Android Gradle Plugin Version` 을 4.2.0으로 설정해준다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/Android%20Studio%20Gradle%20Version.png?raw=true">





### Android Variables(변수) 업데이트

`Android/variables.gradle` 파일에 들어간다.

아래와 같이 변경을 해주면 된다.

```gradle
 ext {
     minSdkVersion = 21
-    compileSdkVersion = 29
-    targetSdkVersion = 29
+    compileSdkVersion = 30
+    targetSdkVersion = 30
+    androidxActivityVersion = '1.2.0'
-    androidxAppCompatVersion = '1.1.0'
+    androidxAppCompatVersion = '1.2.0'
+    androidxCoordinatorLayoutVersion = '1.1.0'
-    androidxCoreVersion =  '1.2.0'
-    androidxMaterialVersion =  '1.1.0-rc02'
-    androidxBrowserVersion =  '1.2.0'
-    androidxLocalbroadcastmanagerVersion =  '1.0.0'
-    androidxExifInterfaceVersion = '1.2.0'
-    firebaseMessagingVersion =  '20.1.2'
-    playServicesLocationVersion =  '17.0.0'
+    androidxCoreVersion = '1.3.2'
+    androidxFragmentVersion = '1.3.0'
-    junitVersion =  '4.12'
-    androidxJunitVersion =  '1.1.1'
-    androidxEspressoCoreVersion =  '3.2.0'
+    junitVersion = '4.13.1'
+    androidxJunitVersion = '1.1.2'
+    androidxEspressoCoreVersion = '3.3.0'
     cordovaAndroidVersion = '7.0.0'
 }
```



# 마무리

지금까지 Capacitor 3.0 버전으로 업데이트 하는 방법과 IOS 및 Android에 적용시키는 방법을 알아봤다.

더 자세한 정보를 얻고 싶다면, [Capacitor 공식 사이트](https://capacitorjs.com/docs) 를 살펴보고, 다양한 Plugin들을 실제 프로젝트에 적용시켜보면서, 제대로 동작하는지 확인해보면 좋을 것 같다.