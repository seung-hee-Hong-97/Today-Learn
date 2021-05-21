#  1. Camera 기능 구현 시 해야 할 기초적인 사항

### Ionic tool 설치

- 터미널에 아래의 명령어를 입력해 `Recourse Generator (자원 생성기)` 를 설치한다.

```bash
$ npm install -g @ionic/cli native-run cordova-res
```

> VsCode와 같은 에디터에 있는 터미널에서 하는 것이 더 편할 수 있다.



- 설치할 때 에러 발생시 [참조](https://ionicframework.com/docs/developing/tips#resolving-permission-errors)



### App 생성

- 터미널에 아래의 명령어를 입력해 Ionic Project를 생성한다.

``` bash
$ ionic start photo-gallery tabs --type=angular --capacitor

$ cd photo-gallery(파일명)
```

> - Angular, Capacitor를 사용하는 Ionic Project가 생성된다.
>
> - photo-gallery 부분은 원하는 파일명을 써주면 된다.



### PWA Elements 설치

- 터미널에 아래의 명령어를 입력해 Camera, Toast와 같은 플러그인들을 실행할 수 있도록 환경을 설정한다.

``` bash
npm install @ionic/pwa-elements
```



- VsCode와 같은 에디터에서 `src/main.ts` 접속하여 아래의 코드를 작성해준다.

```typescript
import {defineCustomelements} from '@ionic/pwa-elements/lolder';

// 플랫폼이 bootstrapped된 후 element loader 호출
defineCustomElements(window);
```



### App 실행

``` bash
$ ionic serve
```

>  웹에서 실행되는 명령어, 만약 Android와 IOS같은 모바일 기기에서 실행하고 싶은 경우 [여기를 참조!](https://github.com/sejong77/Today-Learn/blob/Master/Ionic%20FrameWork/Ionic%20CLI%20%EC%84%A4%EC%B9%98%20%EB%B0%8F%20%EC%8B%A4%ED%96%89%20%EB%A7%A4%EB%89%B4%EC%96%BC.md)



### Photo Gallery

처음 App을 생성할 때 type을 tabs로 생성하였기 때문에 tab1, tab2, tab3, tabs가 포함된 App이 있을 것이다.

편의를 위해 tab2를 Photo Gallery로 만들어본다.



`tab2.page.html`에 접속하여 `<ion-title>` 의 Tab2를 Photo Gallery로 변경시켜준다. (전부 다 변경)

```html
<ion-header>
  <ion-toolbar>
    <ion-title>Tab 2</ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
  <ion-header collapse="condense">
    <ion-toolbar>
      <ion-title size="large">Tab 2</ion-title>
    </ion-toolbar>
  </ion-header>
</ion-content>
```



- 촬영 버튼을 만들기 위해 `<ion-fab>`을 사용한다.

```html
<ion-content>
	<ion-fab verticla="bottom" horiontal="center" slot="fixed">
  	<ion-fab-button>
    	<ion-icon name="camera"></ion-icon>
    </ion-fab-button>
  </ion-fab>
</ion-content>
```



- 그 후 tabs 메뉴의 아이콘과 이름을 바꿔주기 위해 `tabs.page.html`에 접속하여 label과 icon name을 바꿔준다.

```html
<ion-tab-button tab="tab2">
	<ion-icon name="images"></ion-icon>
  <ion-label>Photos</ion-label>
</ion-tab-button>
```



# 2. Camera를 통해 사진 촬영하는 기능 구현

### Photo Service

``` bash
 $ ionic g service services/photo
```

> Src/app/ 위치에 services 폴더가 생성되고 photo.service.ts 파일이 생성된다.



`services/photo.service.ts` 파일에 접속한 후 아래의 코드를 작성

``` typescript
import { Plugins, CameraResutType, Capacitor, FilesystemDirectory,
       CameraPhoto, CameraSource} from '@capacitor/core';

const { Camera, Filesystem, Storage} = Plugins;
```



다음으로 `servies/photo.service.ts` 파일의 PhotoService 클래스 내부에  addNewToGallery 메소드를 생성한다.

```typescript
public async addNewToGallery() {
  // 사진 촬영하는 기능
  const capturePhoto= await Camera.getPhoto({
    resultType: CameraResultType.Uri,
    source: CameraSource.Camera,
    quality: 100
  });
}
```

> Camera.getPhoto() 를 활용하여 Capacitor Camera Plugin을 추상화하여 플랫폼 상관 없이 동일한 코드로 사용할 수 있다.



` tab2.page.ts` 파일에서 addNewToGallery() 메소드를 호출하는 메소드를 만들고,  PhotoService를 import 시켜준다.

``` typescript
import {PhotoService} from '../services/photo.service';

constructor(public photoService: PhotoService) {}

addPhotoToGallery() {
  this.photoService.addNewToGallery();
}
```



촬영 버튼을 누르면 실질적으로 촬영을 할 수 있도록 `tab2.page.html`에 접속한 후, 클릭할 경우addPhotoToGallery() 메소드를 호출하는 코드를 작성한다.

``` html
<ion-content>
	<ion-fab vertical="bottom" horizontal="center" slot="fixed">
  	<ion-fab-button (click)="addPhotoToGallery()">
    	<ion-icon name="camera"></ion-icon>
    </ion-fab-button>
  </ion-fab>
</ion-content>
```

> ionic serve를 통해 브라우저를 실행한 후 촬영 버튼을 누르면 사진을 찍을 수 있는 기능이 구현되었다.



### Displaying Photos (찍은 사진 확인하기)

`photo.service.ts` 파일에서 PhotoService 클래스 외부에 Photo라는 인터페이스를 생성해준다

``` typescript
export interface Photo {
  filepath: string;
  webviewPath: string;
}
```



PhotoService 클래스 가장 상단에 카메라로 찍은 각 사진에 대한 참조를 포함하는 사진 배열을 정의해준다.

``` typescript
export class PhotoService {
  public photos: Photo[]= [];
  
  // 다른 코드들.....
}
```



addNewToGallery 메소드에서 새로 찍은 사진을 사진 배열의 첫 부분에 추가시켜주는 코드를 작성한다.

``` typescript
const capturedPhoto= await Camera.getPhoto({
  resultType: CameraResultType.Uri,
  source: CameraSource.Camera,
  quality: 100
});

this.photos.unshift({
  filepath: "soon....",
  webviewPath: capturedPhoto.webPath
});
```



tab2.page.html 파일에 접속한 후, 찍은 사진들을 화면에 보여주도록 Grid 방식으로 이미지들을 나열 시켜주도록 한다.

``` html
<ion-content>
	<ion-grid>
  	<ion-row>
    	<ion-col size="6" *ngFor="let photo of photoService.photos; index as position">
      	<ion-img [src]="photo.webviewPath"></ion-img>
      </ion-col>
    </ion-row>
  </ion-grid>
  
  <!-- 다른 코드들 -->
</ion-content>
```



이제 찍은 사진들이 Grid 방식으로 나열되는 것을 볼 수 있다. 하지만 웹을 새로고침하거나 닫았다가 다시 열면 저장이 되어 있진 않다. 그렇기 때문에 사진들을 FileSystem에 저장하는 것을 살펴보겠다.



# 3. Filesystem에 찍은 사진들 저장하는 기능 구현

### Filesystem API

먼저 `photo.service.ts` 파일에 있는 PhotoService 클래스에 savePicture() 메서드를 만든다



savePicture() 메서드는 찍은 사진을 나타내는 photo 개체를 전달한다.

``` typescript
private async savePicture(cameraPhoto: CameraPhoto) {}
```



savePicture() 메서드를 addNewToGallery() 메서드에서 호출해서 사용하면 된다.

``` typescript
public async addNewToGallery() {
  // 사진 촬영하는 기능
  const capturedPhoto= await Camera.getPhoto({
    resultType: CameraResultType.Uri,	// 파일 기반 데이터로써, 최고 성능 제공
    source: CameraSource.Camera,	// 카메라로 새로운 사진을 자동으로 찍는 기능
    quality: 100	// 최고의 quality (0 ~ 100)
  });
  
  // 찍은 사진들을 저장하고 사진 모음집에 추가
  const savedImageFile= await this.savePicture(capturedPhoto);
  this.photos.unshift(savedImageFile);
}
```



이제 Capacitor 파일 시스템 API를 활용하여 사진을 파일 시스템에 저장할 것 인데, 우선 사진을 base64 형식으로 변환한 다음 파일 시스템의 writeFile 함수에 데이터를 입력해야 한다.



`tab2.page.html` 파일에서 각 이미지의 원본 경로 (src)를 webviewPath 속성으로 설정하여 각 사진을 화면에 표시한다.

이렇게 하면, 설정한 다음 새로운 사진의 개체를 반환한다.

``` typescript
private async savePicture(cameraPhoto: CameraPhoto) {
  // 사진의 형식을 base64 형태로 바꾸고, 저장하는데 파일 시스템 API가 필요하다.
  const base64Data= await this.readAsBase64(cameraPhoto);
  
  // 데이터 디렉토리에 파일을 작성한다.
  const fileName= new Date().getTime() + '.jpeg';
  const saveFile= await Filesystem.writeFile({
    path: fileName,
    data: base64Data,
    directory: FilesystemDirectory.Data
  });
  
  // 메모리에 이미 load되었기 때문에 base64 대신에 webPath를 통해 새로운 이미지를 보여준다.
  return {
    filePath: fileName,
    webviewPath: cameraPhoto.webPath
  };
}
```



`savePicture()` 메소드에서 사용한 `readAsBase64()`메서드는 소량의 플랫폼 별(웹, 모바일) logic이 필요하기 때문에, 별도의 방법을 통해 구성할 수 있다. 현재는 웹에서 실행할 수 있는 logic이다.

``` typescript
private async readAsBase64(cameraphoto: CameraPhoto) {
  // 사진을 가져오고, blob으로 읽은 다음, base64 형식으로 변환
  const response= await fetch(cameraPhoto.webPath);
  const blob= await response.blob();
  
  // blob으로 읽은 다음, base64 형식으로 변환 후, 문자열 형태로 return 한다.
  return await this.convertBlobToBase64(blob) as string;
}

convertBlobToBase64= (blob:Blob) => new Promise( (resolve, reject) => {
  const reader= new FileReader;
  reader.onerror= reject;
  reader.onload= () => {
    resolve(reader.result);
  };
  reader.readAsDataURL(blob);
});
```

> fecth() 메서드는 파일을 blob 형식으로 읽기 위한 깔끔한 방법으로, FileReader의 readAsDataURL()은 사진 blob을 base64 형식으로 변환한다.



이렇게 하면, 사진을 찍을 때 마다 자동으로 파일시스템에 저장된다.



# 4. Loding Photos from the Filesystem (파일 시스템에서 사진 불러오기)

현재는 파일 시스템에 사진을 촬영 및 저장하는 기능까지 구현되어 있다. 이제는 파일 시스템에 저장되었지만 각 파일에 대한 포인터를 저장하여 사진 갤러리에 다시 표시할 수 있는 방법이 필요하다.

`Capacitor Storage API`를 통해 사진 배열을 `key - value` 저장소에 저장할 수 있다.



### Storage API

저장소의 key로 작용할 상수 변수를 정의한다.



photo.service.ts 파일에서 PhotoService 클래스의 최상단 쪽에 아래의 코드를 작성한다.

``` typescript
export class PhotoService {
  public photos: Photo[]= [];
  private PHOTO_STORAGE: string= "photos";
  
  // 다른 코드들....
}
```



`addNewToGallery()` 메서드 안에 사진 배열을 저장하는 `Storage.set()` 메서드를 만들고 `Storage.set()`에 사진을 저장하게 되면 사진들이 배열 형식으로 저장된다.

이런 방식을 사용하면, 웹을 닫거나 새로고침을 하거나 해도 그대로 저장이 되어 있다.

``` typescript
Storage.set({
  key: this.PHOTO_STORAGE,
  value: JSON.stringify(this.photos)
});
```



사진 배열에 데이터를 저장한 상태에서 해당 데이터를 검색할 수 있는 `loadSave()` 메서드를 만든다.

동일한 key를 사용하여 JSON형식의 사진 배열을 검색한 후, 배열을 분석한다.

``` typescript
public async loadSaved() {
  // 캐시된 사진 배열 데이터 검색
  const photoList= await Storage.get({key: this.PHOTO_STORAGE});
  this.photos= JSON.parse(photoList.value) || [];
}
```



웹에서는 파일 시스템 API가 인덱스를 사용하기 때문에, Photo 객체의 새로운 base64 속성을 사용하여 파일 시스템의 각 이미지를 base64 형식으로 읽어야 한다.

`loadSaved()` 메서드에 다음 코드를 추가시켜준다.

``` typescript
// base64 형식으로 읽어온 사진들을 보여준다.
for(let photo of this.photos) {
  // 파일 시스템으로부터 읽어온 각각의 사진 데이터들을 저장한다.
  const readFile= await Filesystem.readFile({
    path: photo.filePath,
    directory: FilesystemDirectory.Data
  });
  
  // 오직 Web 플랫폼에서는 base64형식의 데이터로 사진을 가져와야 한다.
  photo.webviewPath= `data:image/jpeg;base64, ${readFile.data}`;
}
```



`tab2.page.ts` 파일에 `ngOnInit()` 메소드를 추가시켜주면 찍은 사진들이 저장되고 사용자가 볼 수 있게 된다.

``` typescript
async ngOnInit() {
  await this.photoService.loadSaved();
}
```





# 5. Adding Mobile (모바일에 추가하기)

웹에서 작동하는 Camera 기능을 `Android`,  `IOS`와 같은 모바일에도 적용시킬 수 있다.



### Import Platform API

`Ionic Platform API` 를 import 시켜주면 현재 장치에 대한 정보를 검색하는데 유용하다.

현재 실행중인 플랫폼(웹, 앱)을 기준으로 실행할 코드를 적용시켜주면 된다. 우선, photo.service.ts 파일에 아래와 같은 코드를 추가해준다.

``` typescript
import {Platform} from '@ionic/angular';

export class PhotoService {
  public photos: Photo[]=[];
  private PHOTO_STORAGE: string="photos";
  private platform: Platform;
  
  constructor(platform: platForm) {
    this.platform= platform;
  }
  
  // 다른 코드들....
}
```



### Platform-specific Logic

우선 모바일에서도 사진 저장을 하기 위해 업데이트를 시켜줘야 한다.

`photo.service.ts`  파일에 있는 `readAsBase64` 메소드에서 앱이 실행 중인 플랫폼을 확인한다.

`"hybrid"(Capacitor Or Cordova)` 인 경우 `Filesystem readFile()` 메서드를 확인하여 사진 파일을 base64 형식으로 읽는다. 그렇지 않으면 웹에서 앱을 실행할 때 이전과 동일한 로직을 사용하면 된다.

``` typescript
private async readAsBase64(cameraPhoto: CameraPhoto) {
  // "hybrid"의 의미는 Cordova 혹은 Capacitor 를 사용한다는 뜻
  if(this.platform.is('hybrid')) {
    // 파일을 base64형식으로 읽는다.
    const file= await Filesystem.readFile({
      path: cameraPhoto.path
    });
    return file.data;
  }
  else {
    // 사진을 가져와서 blob으로 읽고, base64형식으로 변환한다.
    const response= await fetch(cameraPhoto.webPath);
    const blob= await response.blob();
    
    return await this.convertBlobToBase64(blob) as string;
  }
}
```



 savePicture() 메서드를 업데이트 한다.

모바일에서 실행을 할 때, filepath를 `savedFile.uri`로 설정하고, webviewPath를 `Capacitor.convertFileSrc()` 메서드로 설정한다. [자세한 정보](https://ionicframework.com/docs/core-concepts/webview#file-protocol) 

``` typescript
// 장치에 파일을 저장한다.
private async savePicture(cameraPhoto: CameraPhoto) {
  // 사진을 base64 형식으로 변환한다. 저장하는데 Filesystem의 API가 필요하다.
  const base64Data= await this.readAsBase64(cameraPhoto);
  
  // data 디렉토리에 파일을 작성한다.
  const fileName= new Date().getTime() + '.jpeg';
  const savedFile= await Filesystem.writeFile({
    path: fileName,
    data: base64Data,
    directory: FilesystemDirectory.Data
  });
  
  if(this.platform.is('hybrid')) {
    // 'file://' 경로를 HTTP로 다시 작성하여 새로운 이미지를 보여준다.
    // Details: https://ionicframework.com/docs/building/webview#file-protocol
    return {
      filePath: fileName,
      webviewPath: Capacitor.convertFileSrc(savedFile.uri)
    }; 
  }
  else {
    // 메모리에 이미 로드되었으므로 base64 대신 webPath를 사용하여 새 이미지 표시
    return {
      filepath; fileName,
      webviewPath: cameraPhoto.webPath
    };
  } 
}
```



다음에는 전에 웹에 대해 구현한 `loadSaved()` 메서드로 간다. 모바일에서는 이미지 태그의 source를 Filesystem의 각 사진 파일에 직접 설정하여 자동으로 표시할 수 있다. 



따라서 웹에서만 Filesystem의 각 이미지를 base64 형식으로 읽어주면 된다. 

이 기능을 업데이트 하기 위해, Filesystem 코드 주위에 if 문을 추가해준다.

``` typescript
public async loadSaved() {
  // 캐시된 사진 배열의 데이터를 검색
  const photoList= await Storage.get({key: this.PHOTO_STORAGE});
  this.photos= JSON.parse(photoList.value) || [];
  
  // 웹에서 실행할 때 가장 쉽게 탐지할 수 있는 방법으로는 hybrid가 아니면 이런식으로 하세요 라는 코드를 작성한다.
  if(this.platform.is('hybrid') == false) {
    // base64 형식으로 읽어서 사진을 보여준다.
    for(let photo of this.photos) {
      // Filesystem으로부터 각각의 사진 데이터들을 읽어서 저장한다.
      const readFile= await Filesystem.readFile({
        path: photo.filepath,
        directory: FilesystemDirectory.Data
      });
      
      // 오직 플랫폼이 웹일 경우, 사진을 base64형식으로 가져온다.
      photo.webviewPath= `data:images/jpeg;base64, ${readFile.data}`;
    }
  }
}
```



이제 web, android, iOS 에서 실행되는 Camera 기능을  하나의 코드베이스로 구성할 수 있게 되었다.



# 6. Deploying to IOS and Android (IOS와 Android에 배포하기)

### Capacitor Setup

Capacitor는 `iOS`, `Android` 등의 기본 플랫폼에 웹, 앱을 쉽게 배포할 수 있는 Ionic의 공식 앱 런타임이다.

만일 `ionic serve`를 통해 계속해서 서버와 연동이 되어 있다면 취소하고, 터미널에 `ionic build`를 입력해 ionic project를 새롭게 build시켜준다. 그 후에 ios와 android를 각각 터미널에 `ionic cap add ios`,  `ionic cap add android`를 통해 생성해준다.



생성을 완료했다면 프로젝트의 `root`부분에 android와 ios가 각각 생성되었을 것이다.

`웹 디렉토리(기본 값: WWW)`를 업데이트 하는 build를 할때 마다 이러한 변경 사항을 기본 프로젝트에 복사해야 한다



터미널에 아래의 명령어를 입력해 복사 및 업데이트를 해준다.

``` bash
$ ionic cap copy

$ ionic cap sync
```



### Android Deployment (안드로이드에 배포)

Capacitor Android 앱은 `Android Studio`를 통해 구성 및 관리가 된다.



우선 터미널에 아래의 명령어를 입력해 Android Studio에서 Native Android 프로젝트를 실행한다.

```bash
$ ionic cap open android
```



Android Studio에 들어가서 `AndroidMainifest.xml` 파일을 연다.



AndroidMainifest.xml 파일에 `<Permissons>` 부분으로 가서, 아래의 코드가 있는지 확인한다.

``` xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> 코드가 있는 것을 확인했다면, Android Studio에서 AVD를 설정하고, Run을 시켜 정상적으로 작동하는 것을 확인한다.



### IOS Deployment (iOS 배포)

Capacitor IOS 앱은` Xcode`를 통해 구성 및 관리가 되며, 의존성은 `CocoaPods`를 통해 관리된다.



우선 터미널에 아래의 명령어를 통해 Xcode에서 Native IOS 프로젝트를 실행한다.

``` bash
$ ionic cap open ios
```

>  일부 Native Plugins 이 작동하려면 사용자 권한이 구성되어야 한다.



IOS는 `Camera.getPhoto()`가 처음 호출된 후 자동으로 `modal dialog`를 보여준다.

![Xcode Custom iOS Target Properties](https://ionicframework.com/docs/assets/img/guides/first-app-cap-ng/xcode-info-plist.png)

``` bash 
Xcode에서 App의 Info 부분 접속 -> Privacy - Camera Usage Description 우 클릭 -> Raw Keys & Values 클릭을 하면 위의 사진처럼 나올 것이다. 사진처럼 있는 것을 확인했다면 Signing & Capabilities 부분에 접속한다.

Team과 Bundle idenitifer를 설정한다. 그 후 Run을 하면 정상적으로 작동하는 것을 볼 수 있다.
```



# 7. 실시간 Reload 기능을 활용하여 신속한 앱 개발

### Live Reload

Live Reload를 사용해 사진 갤러리 기능 중에, 삭제 기능을 구현할 수 있다. 먼저 원하는 플랫폼 (Ios, android)을 선택하고 터미널에 다음 명령어를 실행

``` bash
# Android
$ ionic cap run android -l --external

# IOS
$ ionic cap run ios -l --external
```



### Deleting Photos

`tab2.page.html` 파일을 열고 `<ion-img>` 태그에 click handler를 추가 시켜준다.



이미지를 클릭하면 삭제 혹은 닫기 의 대화상자가 나타나게끔 한다.

``` html
<ion-col size="6" *ngFor= "let photo of photoService.photos; index as position">
	<ion-img [src]="photo.webviewPath" (click)="showActionSheet(photo, position)"></ion-img>
</ion-col>
```



 `tab2.page.ts` 파일을 열고 다음의 코드를 추가시켜준다.

``` typescript
import {ActionSheetController} from '@ionic/angular';

constructor(public photoService: PhotoService,
           	public actionSheetController: ActionSheetController) {}
```



기존에 `tab2.page.ts` 파일에 있던 `import { PhotoService } from 'src/app/services/photo.service'; ` 

부분에 Photo를 추가 시켜준다.

``` typescript
import {Photo, PhotoService} from 'src/app/services/photo.service'; 
```



다음에는 `showActionSheet()` 메서드를 구현한다. 두가지 옵션으로  PhotoService에 `delectPicture()` 메서드와 `Cancel`을 추가한다. Cancel을 호출하면 자동으로 actionSheet가 닫히도록 한다. (tab2.page.ts에 작성)

``` typescript
public async showActionSheet(photo: Photo, position: number) {
  const actionSheet= await this.actionSheetController.create({
    header:'Photos',
    buttons:[{
      text:'Delete',
      role:'destructive',
      icon:'trash',
      handler:()=> {
        this.photoService.deletePicture(photo, position);
      }
    }, {
      text:'Cancel',
      role:'cancel',
      icon:'close',
      handler: () => {
       // actionSheet이 자동으로 닫히도록 아무것도 쓰지 않는다.
      }
    }]
  });
  
  await actionSheet.present();
}
```



삭제를 클릭 했을 경우 삭제를 하는 기능을 구현하기 위해 `photo.service.ts` 파일에 `deletePicture()` 메서드를 만들어줘야 한다.

``` typescript
public async deletePicture(photo: Photo, position: number) {
  // 선택한 사진을 사진 배열에서 제거해준다.
  this.photos.splice(position, 1);
  
  // 기존 사진 배열에 캐시된 사진 배열을 덮어 씌워 업데이트 한다.
  Storage.set({
    key: this.PHOTO_STORAGE,
    value: JSON.stringify(this.photos)
  });
  
  // 파일 시스템으로부터 사진 파일이 삭제된다.
  const fileName= photo.filepath.substr(photo.filepath.lasIndexOf('/') + 1);
  
  await Filesystem.deleteFile({
    path: fileName,
    directory: FilesystemDirectory.Data
  });
}
```



이제 선택한 사진이 먼저 사진 배열에서 제거된다. 그 후에 `Capacitor Storage API`를 사용하여 캐시된 버전의 사진 배열을 업데이트 해준다. 마지막으로 `Filesystem API`를 통해 실제 사진 파일 자체를 삭제하는 과정을 통해 카메라의 기능을 어느정도 구현했다.



