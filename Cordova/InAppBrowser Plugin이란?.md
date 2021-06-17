# cordova-plugin-inappbrowser

Cordova Plugin들중 하나로서, 앱 내에서 유용한 기사 혹은 비디오 및 웹 리소스를 표시할 때 사용한다.

사용자는 앱에서 나가지 않더라도 웹 페이지를 볼 수 있다.



## 설치 방법

우선 전제조건으로 Ionic Framework를 사용한다고 가정하고 진행

Mac OS 기준으로 터미널에 아래의 명령어를 입력하여 설치한다.



### Capacitor

```bash
$ npm install cordova-plugin-inappbrowser

$ npm install @ionic-native/in-app-browser

$ npx cap sync

$ ionic cap sync
```



### Cordova

```bash
$ ionic cordova plugin add cordova-plugin-inappbrowser

$ npm install @ionic-native/in-app-browser
```



> 설치가 완료됐다면, `app-module.ts` 파일의 Providers 부분에 `InAppBrowser` 를 추가시켜주면 정상적으로 사용할 수 있다.



## 사용 방법

InAppBrowser 플러그인이 정상적으로 설치됐다면, InAppBrowser를 생성할 수 있다.



### create

- create(url: `string`, target?: `string`, options?: `string` | *InAppBrowserOptions*)

> `url`: 로드할 URL 주소를 입력
>
> `target`: URL을 로드할 대상으로, 기본 값은 _self 이다.
>
> - `_self` : URL이 화이트리스트(명시적으로 허가된 목록)에 있으면 Cordova WebView에서 열리고, 그렇지 않으면 InAppBrowser에서 열림
> - `_blank` : 무조건 InAppBrowser에서 열림
> - `_system` : 시스템의 웹 브라우저에서 열림 
>
> `options`: InAppBrowser에 대한 옵션으로 선택사항이다. 옵션 문자열에는 공백이 없어야하며, 각 기능의 이름 / 값 쌍은 쉼표로 구분해야 한다. 또한 기능의 이름은 대, 소문자를 구분하지 않는다.
>
> - [InAppBrowserOptions 정보1](https://ionic.io/docs/premier-plugins/inappbrowser#inappbrowseroptions)
>
> - [InAppBrowserOptions 정보2](https://cordova.apache.org/docs/en/10.x/reference/cordova-plugin-inappbrowser/#preferences)



``` typescript
// 예시 코드

import { InAppBrowser } from '@ionic-native/in-app-browser/ngx';

constructor(private inappBrowser: InAppBrowser) { }

const browser = this.inappBrowser.create(url, target, options);

browser.on('loadstart' | 'loadstop' | 'loaderror' | 'message' | 'beforeload' | 'exit');

browser.close();
```

> `on` : addEventListener와 비슷한 역할을 하는데, 브라우저의 상태에 따라 이벤트를 추가할 수 있다.
>
> `close` : 브라우저를 닫는다.



## 참조 문서

- [참조 문서 1](https://github.com/apache/cordova-plugin-inappbrowser/#cordova-plugin-inappbrowser)

- [참조 문서 2](https://cordova.apache.org/docs/ko/3.1.0/cordova/inappbrowser/inappbrowser.html)

- [참조 문서 3](https://ionicframework.com/docs/native/in-app-browser)

