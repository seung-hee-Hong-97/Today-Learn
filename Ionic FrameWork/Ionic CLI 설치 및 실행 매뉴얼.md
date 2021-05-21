# Ionic CLI 설치 매뉴얼



## 1. Node.js 설치 

- [다운로드 하는 곳](https://nodejs.org/ko/download/)



- 위의 링크로 들어가서 각자에 맞는 OS를 선택하여 설치한다.



- 설치가 완료되었다면, Window는 PowerShell 혹은 명령 프롬프트에 접속하고, Mac인 경우는 내장 터미널 앱에 접속한다.



- 기본적으로 Window, Mac에 있는 각각의 Shell, Terminal에서 진행하지만, Visual Studio Code와 같은 에디터에서 터미널을 실행하여 진행해도 무방하다.



##  2. Node.js 설치가 완료되었는지 확인

- 기본적으로 node.js를 다운 받았다면, npm도 같이 설치 되었을 것이다.



- 터미널에 접속하여 아래처럼 실행한다.

```bash
$ node --version		
$ npm --version
```

> 각각에 맞는 버전이 나오면 성공적으로 설치가 된 것이다.



## 3. Git 설치하기

- [Git 다운로드](https://git-scm.com/downloads)



- 위의 링크로 들어가서 각자에 맞는 OS를 선택하여 설치한다.



- 설치가 완료되었다면, Window는 PowerShell 혹은 명령 프롬프트에 접속하고, Mac인 경우는 내장 터미널 앱에 접속한다.



- 터미널에 접속하여 아래처럼 실행한다.

``` bash
$ git --version
```

> 버전이 나오면 성공적으로 설치가 된 것이다.



## 4. Ionic CLI 설치하기

### Ionic CLI 설치

- 터미널에 접속하여 아래의 명령어를 입력해 Ionic CLI를 설치한다.

``` bash
$ npm install -g @ionic/cli
```



- 만약 이미 Ionic CLI가 설치된 상태라면, 아래의 명령어를 실행하여 삭제하고 다시 설치한다.

``` bash
$ npm uninstall -g @ionic/cli 
```



### Ionic CLI 설치 에러 시 참고

- 설치가 정상적으로 완료되어야 하지만, EACESS permission 에러가 발생한다면 [이 곳을 참조](https://ionicframework.com/docs/developing/tips#resolving-permission-errors)



- 위의 방식대로 했는데도 불구하고, Ionic CLI가 설치되지 않았을 경우, 터미널에 접속하여 , $ cd ~ 를 통해 홈 디렉토리로 이동한 다음, $ cd /usr/local 로 이동한 다음, 그 곳에서 $npm install -g @ionic/cli를 통해 설치한다.

```bash
$ cd ~			# 홈 디렉토리로 이동

$ cd /usr/locacl		

$ npm install -g @ionic/cli			### usr/local 위치에 @ionic/cli 설치
```



## 5. App 생성, 시작하기

### Angular 설치 (만약 ionic start --type을 통해 생성할꺼면 생략 가능)

- Ionic5는 Front-end FrameWork인 Angular, React, Vue와 같이 사용할 수 있다.



- Angular와 Ionic을 같이 사용할 것이므로 Angular를 설치해줘야 한다.



- 터미널에 접속하여 아래의 명령어를 실행하여 설치해준다.

``` bash
$ npm install @ionic/angular$latest --save
```



- 위의 방법으로 설치가 안된다면 아래의 명령어를 통해 설치해준다.

``` bash
$ npm install -g @angular/cli
```



- 그 후에, 아래의 명령어를 실행하여 이미 존재하는 Angular 프로젝트에 ionic을 추가해준다.

``` bash
$ ng add @ionic/angular
```



### ionic start --type을 통한 App 생성

- Ionic 앱을 실행할 때 일반적으로 3가지의 starter가 있다. (1. blank  2. tabs  3. sidemenu)

  1. blank는 아무 것도 없는 빈 템플릿

  2. tabs는 화면 하단에 tabs 메뉴가 생겨 tab1, tab2, tab3을 선택할 수 있도록 구성되어 있다.

  3. sidemenu는 화면 왼쪽 사이드에 메뉴가 생긴다.

  > => 만약 더 많은 템플릿을 알고 싶다면 [여기를 참조!](https://ionicframework.com/docs/v3/cli/starters.html)

  

- 3가지의 템플릿 중 필요하거나 마음에 드는 템플릿을 선정한다.



- 터미널에 접속하여 아래의 명령어를 실행한다. 

```bash
# myApp 위치에는 본인이 원하는 App이름을, blank 위치에는 3가지 템플릿 중 본인이 원하는 템플릿 명을 써주면 된다.
# --type은 angular, react, ionic-angular, ionic1 중에 선택하면 된다.

$ ionic start myApp blank --type=angular --capacitor 
```

> => ionic start 관련하여 더 많은 정보를 얻고 싶을 경우 ([정보얻기](https://ionicframework.com/docs/developing/starting))
>
> => cordova를 사용해도 되지만 이 설치 매뉴얼에서는 capacitor를 사용해서 생성 할 것이다.



- 이제 본인이 설정한 템플릿과 이름을 가진 App이 생성 되었을 것이다.



- visual studio code와 같은 에디터에 접속하여, package.json 파일을 살펴보면, 설치된 것들의 버전을 확인할 수 있다.



- 그 후에, 아래의 명령어를 터미널에 입력하여 웹 자산을 컴파일하고, 배포를 준비한다.

``` bash
$ ionic build
```



## 6-1. App 실행하기 (Web에 실행)

- 이 전에 생성한 App 파일에 접속을 해야 한다. 



- 터미널에서 아래의 명령어를 통해 myApp 폴더에 들어간다.

``` bash
# myApp 위치에는 본인이 생성한 App의 이름을 써주면 된다.
$ cd myApp 
```



- 아래의 명렁어를 실행하면 웹 브라우저에 본인이 생성한 App이 실행된다.

```bash
$ ionic serve				// localhost: 8100
```



- 만약 변경사항, 업데이트 한 것을 실시간으로 확인하고 싶을 경우, 웹 서버와 연결되어 있다면 연결을 끊고 아래의 명령어를 터미널에 입력한다.

``` bash
$ ionic serve -l --external
```



## 6-2. Android를 통해 실행하기

- [Android Studio 설치](https://developer.android.com/studio?hl=ko)
- Android Studio를 설치 완료 후에, 아래의 명령어를 터미널에 입력해 android를 추가 시켜준다.

``` bash
 $ ionic cap add android
```

>  Editor를 확인해보면, android 폴더가 추가 된 것을 볼 수 있다.



- 웹 디렉터리(기본값: www)를 업데이트하는 빌드(ionic build)를 수행할 때마다 이러한 변경 사항을 기본 프로젝트에 복사해야 한다. 아래의 명령어를 터미널에 입력한다.

```bash
$ ionic cap copy
```



- 아래의 명렁어를 터미널에 입력해 ionic 프로젝트를 복사 및 업데이트 시켜준다.

```bash
 $ ionic cap sync android
```



- 아래의 명령어를 터미널에 입력하고, Android Studio에 들어간 후 Run App을 한다. (초록색 세모 버튼)

``` bash
$ ionic cap run android
```

>  AVD를 통해 정상적으로 Android App이 실행된 것을 볼 수있다.



- 만약 변경된 사항을 AVD를 통해 바로바로 확인하고 싶을 경우, AVD가 실행중이라면 Stop App 버튼을 누른 후 (빨간색 네모 버튼) 아래의 명령어를 터미널에 입력한다. 

``` bash
$ionic cap run android -l --external
```

> Android Studio에 들어간 후 Run App을 하면  App을 계속해서 재부팅하지 않아도, 변경 사항을 저장하면 실시간으로 적용된다.



## 6-3. IOS를 통해 실행하기

- iOS 개발을 하려면 맥북과 Xcode를 설치해야 한다. 기본적으로 Xcode는 맥북의 App Store에서 다운 받을 수 있지만, 많은 시간이 걸리므로 AppleDeveloper Site에서 다운 받는 것이 더 편리 하다. (기본적으로 설치 시간이 조금 걸린다.)



- Xcode 설치 방법
  1. [AppleDeveloper Site 접속](https://idmsa.apple.com/IDMSWebAuth/signin?appIdKey=891bd3417a7776362562d2197f89480a8547b108fd934911bcbea0110d07f757&path=%2Fdownload%2Fmore%2F&rv=1) (Apple ID로 로그인)
  2. 다운 받을 Xcode 버전 검색
  3. Xcode xip 압축파일 다운로드
  4. 다운 받은 xip 파일 실행
  5. 압축이 해제된 Xcode 파일 응용프로그램 폴더로 복사
  6. Xcode 실행



- Xcode 설치가 완료 되었다면, 아래의 명령어를 터미널에 입력해 ios를 추가 시켜준다.

``` bash
$ ionic cap add ios
```

> Editor를 확인해보면, ios 폴더가 추가 된 것을 볼 수 있다.



- 웹 디렉터리(기본값: www)를 업데이트하는 빌드(ionic build)를 수행할 때마다 이러한 변경 사항을 기본 프로젝트에 복사해야 한다. 아래의 명령어를 터미널에 입력한다.

```bash
$ ionic cap copy
```



- 아래의 명렁어를 터미널에 입력해 ionic 프로젝트를 복사 및 업데이트 시켜준다.

```bash
$ ionic cap sync ios
```



- 아래의 명령어를 터미널에 입력하고, Xcode에 들어간 후 Run App을 한다. (검은색 세모 버튼)

```bash
$ ionic cap run ios
```

> 시뮬레이터를 통해 정상적으로 앱이 작동하는 것을 볼 수 있다.



- 만약 변경된 사항을 Simulator를 통해 바로바로 확인하고 싶을 경우, Simulator가 실행중이라면 Stop App 버튼을 누른 후 (검은색 네모 버튼) 아래의 명령어를 터미널에 입력한다. 

``` bash
$ ionic cap run ios -l --external
```

> Xcode에 들어간 후 Run App을 하면  App을 계속해서 재부팅하지 않아도, 변경 사항을 저장하면 실시간으로 적용된다.