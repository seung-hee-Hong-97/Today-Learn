# Craco를 통한 절대 경로 설정

프로젝트를 진행하다 규모가 커지게 되면, 디렉토리 구조가 깊숙하게 형성되어 기본적으로 상대경로로 표시되기 때문에 아래와 같이 깔끔하지 못하게 표시 됩니다.

``` typescript
import Header from '../../components/Button/Button.tsx';
```

> 따라서 절대경로를 설정하여 `@components/Button/Button.tsx` 와 같이 설정하는 것이 좋습니다.

절대경로 설정을 하는 법을 알아보기 전에, Craco가 무엇인지, eject 명령어를 통해 숨겨둔 폴더들을 노출 시킬 경우 발생하는 상황 등에 대해 알아보겠습니다.



## eject

React 프로젝트를 설치할 때 CRA(create-react-app)을 통해 설치하게 되는데, CRA는 기본적으로 프로젝트 디렉토리를 간결하게 유지하기 위해 WebPack 설정이나 script 폴더들을 숨겨둡니다.

하지만 이를 커스텀해야 할 경우 `eject` 명령어를 통해 추출할 수 있는데, 이렇게 추출하게 되면 디렉토리 상에 모두 노출이 되고 이전 상태로 돌아갈 수 없게 됩니다.

``` bash
$ npm run eject
```

> 위의 명령어를 통해 숨진 폴더들을 노출시킬 수 있습니다.

### CRA 기본 디렉토리 구조

<img src="https://blog.kakaocdn.net/dn/bKU9Et/btrq4wSDIzQ/TBEdWkYdJwVISRG23HodN0/img.png">

### eject 명령어 실행 후 디렉토리 구조

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Feoit8C%2Fbtrq23XOPtp%2FF3g6z3khxNEgvA5wcXUmM1%2Fimg.png">



## Craco

Craco는 Create-React-App Configuration Override의 약어로, CRA에 config 설정을 덮어쓰기 위한 패키지 입니다.



### 1. 설치

``` bash
$ npm install @craco/craco
$ npm install craco-alias -D
```

> crac-alias 설치 시 `-D`를 추가하는 이유는 해당 프로젝트를 개발할 때만 필요한 의존성을 `dvDependencies`에 정의하기 위함입니다.



### 2. package.json 수정

`scripts` 부분을 수정합니다.

``` json
// package.json

"scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test",
  "eject": "react-scripts eject"
}
```



### 3. craco.config.js 파일 생성

`craco.config.js` 파일의 생성 위치는 root 디렉토리 하위, 쉽게 말하면 `src` 폴더와 같은 위치에 생성하면 됩니다.

<img src="https://github.com/sejong77/Today-Learn/blob/Master/image/craco%ED%8F%B4%EB%8D%94%EC%9C%84%EC%B9%98.png?raw=true">



### 4. Craco.config.js 파일 설정

``` javascript
// Craco.config.js

import CracoAlias from 'craco-alias';  // ES6 이상일 경우
const CracoAlias = require('craco-alias');  // ES6 미만일 경우

module.exports = {
  plugins: [
    {
      plugin: CracoAlias,
      options: {
        source: 'tsconfig',
        tsConfigPath: './tsconfig.paths.json',
      },
    },
  ],
};
```

> 1. CRA + Typescript를 기반으로 프로젝트를 진행하는 경우 생성한 `tsconfig.json` 파일을 `source` 부분에 선언
> 2. `tsConfigPath` 부분에 선언 할 `tsconfig.paths.json` 파일은 아래 글을 참조해주세요.강긴



### 5. tsconfig 파일 설정

``` json
// tsconfig.json

{
  ...
  "extends": "./tsconfig.paths.json",
  ...
  
  "include": ["./src/"],
  "exclude": ["node_modules/*"]
}
```

> 기존 설정에 위의 코드만 추가해주면 됩니다.



### 6. tsconfig.paths.json 파일 생성 및 설정

`tsconfig.paths.json` 파일은 절대경로를 설정하는 파일입니다.

이 파일은 `tsconfig.json` 파일과 동일한 위치에 있습니다.

본인이 지정하려는 파일을 `paths`에 추가하여 아래의 예시와 같이 절대경로로 설정하면 됩니다.

``` json
// tsconfig.paths.json

{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@api/*": ["src/api/*"],
      "@components/*": ["src/components/*"],
      "@configs/*": ["src/configs/*"],
      "@contexts/*": ["src/Contexts/*"],
      "@hooks/*": ['src/hooks/*'],
      "@interfaces/*": ["src/interfaces/*"],
      "@layouts/*": ["src/layouts/*"],
      "@lib/*": ["src/lib/*"],
      "@pages/*": ["src/pages/*"]
    }
  }
}
```



### 7. 절대 경로 설정

위의 과정을 모두 마쳤다면 상대경로로 설정하는 것이 아닌, 절대경로로 설정을 할 수 있습니다.

``` typescript
// 절대경로로 설정
import Button from '@components/Button/Button';
import SubTitle from '@components/SubTitle/SubTitle';
```

> 위에 작성한 상대경로 보다 훨씬 간결하게 작성할 수 있습니다.