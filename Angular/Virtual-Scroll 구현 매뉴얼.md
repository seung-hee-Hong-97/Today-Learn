## 1. 가상 스크롤이란?

Angular에서 제공하는 Scrolling 패키지 중에 Virtual Scroll 기능이 있습니다.

화면에 맞는 항목만 렌더링하여 많은 요소 목록을 성능에 맞게 표시할 수 있습니다.

몇 십개 정도의 적은 데이터는 성능상으로 큰 문제점이 없을 수 있지만 몇 백개, 몇 천개같이 많은 데이터 요소를 로드할 경우 브라우저나 앱의 성능이 많이 느려 질 수 있습니다. 따라서 가상 스크롤을 활용하여 컨테이너 요소의 높이를 렌더링할 총 요소 수의 높이와 동일하게 만든 다음 뷰에 있는 항목만 렌더링하여 렌더링 되는 모든 항목을 시뮬레이션 할 수 있습니다.

정해진 양의 요소를 렌더링한 다음 끝에 도달할 경우 나머지를 렌더링 하는 방식인 무한 스크롤(Infinite-Scroll)과는 다른 방식입니다.



## 2. Angular/CDK 설치 유무 확인

@angular/cdk가 설치되어 있는지 확인하는 방법은 2가지가 있습니다.

**첫 번째 방법**

```bash
$ ng version
```

> 위 명령어를 통해 Angular와 관련된 도구들의 버전을 확인할 수 있습니다.



**두 번째 방법**

`package.json` 파일에 @angular/cdk가 있는지 확인



기본적으로 Angular의 ScrolloingModule은 `@angular/cdk/scrolling` 경로에 있습니다.

따라서 @angular/cdk 가 설치되어 있어야 하는데, 만약 설치되어 있지 않다면 다음 명령어를 입력하여 설치 합니다.

```bash
$ npm install @angular/cdk --save
```

> 설치 할 때 버전 관련 에러가 생길 경우, Angular의 버전이 낮아서 발생하는 경우입니다.
>
> 따라서 Angular와 관련된 주요 도구들의 버전을 업데이트 시켜주면 해결됩니다.



설치가 되었다면 `package.json` 파일에 아래의 이미지와 같이 @angular/cdk가 정상적으로 나타날 것 입니다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/img-angular-cdk.png?raw=true">



## 3. module.ts 파일에 설치한 ScrollingModule을 import에 추가

예를 들어 home 화면에 `Virtual Scroll` 을 적용하는 것을 가정

```typescript
// home.module.ts

// Code...

import { ScrollingModule } from '@angular/cdk/scrolling';
import { ScrollingModule as ExperimentalScrollingModule } from '@angular/cdk-experimental/scrolling';

@NgModule({
  imports: [
    // ...
    ScrollingModule,
    ExperimentalScrollingModule
  ],
})

export class HomePageModule {}
```



## 4. Virtual Scroll 적용

### 1. 테스트를 위해 10000개의 항목을 생성합니다.

```typescript
// home.page.ts

export class HomePage {
  numbers: number[] = [];
  
  constructor() {
    for (let i = 0; i < 10000; i++) {
      this.numbers.push(i);
    }
  }
}
```

### 2. home.page.html파일에 항목을 렌더링합니다.

```html
<cdk-virtual-scroll-viewport id="item-container" itemSize="20" autosize>
 <div *cdkVirtualFor = "let number of numbers; templateCacheSize: 0">
   <p>
     {{ number }}
   </p>
  </div>
</cdk-virtual-scroll-viewport>
```

> - Angular에서 *ngFor을 활용하여 데이터를 렌더링 하는 방식 대신, *cdkVirtualFor을 활용해야 합니다.
>
> - templateCacheSize: 0을 하면 캐싱이 비활성화 됩니다. 이렇게 하는 이유는 templateCache에 너무 많은 메모리를 소비하지 않도록 하기 위함입니다.

### 3. 높이 지정

Virtual Scroll을 완벽하게 구현하기 위해서는 컨테이너의 높이를 지정해야 합니다.

```scss
#item-container {
  height: 500px;
}

div {
  margin-bottom: 10px;
}

p {
  margin: 0;
  text-align: center;
  padding: 25px;
  border-radius: 5px;
  font-size: 32px;
}
```

> 컨테이너(item-container)의 높이를 지정하는 것은 필수적으로 해야하고 나머지 스타일링은 선택사항입니다.

### 4. [테스트 결과](https://stackblitz.com/edit/angular-ivy-4gevhs?file=src%2Fapp%2F)



## 참조 문서

- [Angular Material 공식문서](https://material.angular.io/cdk/scrolling/overview)