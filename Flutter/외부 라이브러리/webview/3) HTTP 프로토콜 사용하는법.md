# HTTP 프로토콜 사용하는 법

인터넷과 관련된 작업을 할 때 해줘야 할 작업이 있는데 바로 HTTP 프로토콜을 Android, IOS 플랫폼에 http를 사용하겠다고 설정을 해줘야 합니다.

왜냐하면 `http`는 보안적인 이슈로 인해 플랫폼에서 허용을 하지 않고 있기 때문입니다.

옛날에는 `http://` 를 통해서 사이트를 사용했는데, 요즘은 대부분이 `https://` 를 통해서 사이트를 사용합니다.

> 인터넷과 관련된 작업은 웹뷰를 띄우는 것 말고도 서버에 요청을 하거나 Url을 사용하는 상황을 모두 포함한 작업입니다.

<br />

## IOS에서 HTTP 프로토콜 사용하는 법

```
프로젝트 > ios > Runner > info.plist
```

> 위의 경로대로 info.plist 파일을 엽니다.

<br />

`info.plist` 파일을 열면 많은 설정이 적혀있는데 맨 밑으로 내러서 `</dict>` 위에 들여쓰기 해서 아래와 같이 작성합니다.

```
  <key>NSAppTransportSecurity</key>
  <dict>
    <key>NSAllowsLocalNetworking</key>
    <true/>
    <key>NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
  </dict>
</dict>  -> 이거 바로 위에 작성하면 됨
</plist>
```

> 위와 같이 설정하면 `http` 프로토콜을 사용할 수 있습니다.
>
> 그런데 이러한 설정은 Flutter만의 설정이 아니라 IOS 네이티브 앱을 개발할 때도 적용되는 사항입니다.

<br />

## Android에서 HTTP 프로토콜 사용하는 법

```
프로젝트 > android > app > src > main > AndroidManifest.xml
```

> 위의 경로대로 AndroidManifest.xml 파일을 엽니다.

<br />

AndroidManifest.xml 파일의 `<mainifest>` 태그 바로 아래에 다음과 같이 추가해줍니다.

``` xml
<uses-permission android:name="android.permission.INTERNET" />
```

그 후에 바로 아래에 있는 `<application>` 태그 안에 아래와 같이 추가해줍니다.

``` xml
<application
     android:usesCleartextTraffic="true">
</application>
```