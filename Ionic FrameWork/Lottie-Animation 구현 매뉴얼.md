## 1. Lottie 설치

``` bash
$ npm install lottie-web ngx-lottie
```



## 2. Lottie가 정상적으로 설치되었는지 확인

`package.json` 파일과 `package-lock.json` 파일을 확인하여, 아래의 이미지처럼 설치가 되었는지 확인

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/package-json(lottie).png?raw=true">

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/package-lock.json(lottie-web).png?raw=true">



<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/package-lock.json(ngx-lottie).png?raw=true">

> 정상적으로 설치가 된 것을 확인했다면 다음으로 넘어간다.



## 3. module.ts 파일에 설치한 Lottie를 import에 추가

예를 들어 home 화면에 `Lottie-Animation` 효과를 적용하는 것을 가정

``` typescript
// home.module.ts

// Code..

import { LottieModule } from 'ngx-lottie';
import { player } from 'lottie-web';

export function playerFactory() {
  return player;
}

@NgModlue({
  imports: [
    LottieModule.forRoot({ player: playerFactory }),
    CommonModule,
    FormsModule,
    // ...
  ],
})

export class HomePageModule {}
```



## 4. 적용하고 싶은 Lottie-Animation 다운로드

적용하고 싶은 Lottie-Animation은 [여기](https://lottiefiles.com/)서 `Lottie JSON` 타입으로 다운로드 받으면 된다.

접속하고 로그인 후에 원하는 애니메이션을 검색하고, 다운로드 받는다.

저장 경로는 아래의 이미지와 같이 `assets` 폴더 하위에 저장한다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/lottie-json%20%EC%A0%80%EC%9E%A5%20%EA%B2%BD%EB%A1%9C.png?raw=true">





## 5. Lottie-Animation 적용

``` html
<!-- home.page.html -->

<ion-header>
	<ion-toolbar>
  	<ion-title> Lottie-Animation 적용해보기 </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
	<ng-lottie [options]="options"> </ng-lottie>
</ion-content>
```

> CSS 설정은 생략



``` typescript
// home.page.ts

import { AnimationOptions } from 'ngx-lottie';

@Component({
  // ..
})

export class HomePage {
  options: AnimationOptions = {
    path: 'assets/lottie/trophy.json',
    autoplay: true,
    loop: true,
  };
}
```

