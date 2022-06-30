# [React] 설치 및 개발환경 구축

MacOS에서 React 설치 및 개발환경 구축하는 방법에 대해 작성할 예정입니다.

## 1. Node.JS 설치

`Node.JS`가 설치되어 있다면 (1) 단계는 넘어가면 됩니다.

2022년 6월 기준

`가장 최신 버전` : 18.3.0 (includes npm 8.11.0)

`최신 LTS 버전` : 16.15.1 (includes npm 8.11.0) 

[Node.JS 다운로드](https://nodejs.org/ko/download/) 를 클릭하여 본인의 OS에 맞는 LTS 버전을 다운로드하면 됩니다.

저는 MacOS를 사용하고 있기 때문에 macOS Installer를 클릭하여 다운로드 했습니다.

<Img src="https://github.com/sejong77/Today-Learn/blob/Master/image/node.js%20%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C%20%ED%99%94%EB%A9%B4.png?raw=true">

설치가 완료 되었다면 터미널에 아래의 명령어를 입력하여 Node.JS가 제대로 설치되었는지 확인합니다.

``` bash
$ node -v
v16.15.1 

# 다운로드 받은 버전이 떠야 정상적으로 설치가 된 것 입니다.
```



## 2. 원하는 위치에 프로젝트 폴더 생성

본인이 원하는 위치에 React 프로젝트를 실행 할 폴더를 생성합니다.



## 3. ReactJS 설치 및 실행

ReactJS를 설치할 수 있는 방법은 `npm` 과 `npx` 를 사용하는 2가지 방법이 있습니다.

`yarn` 을 사용하여 설치할 수도 있지만 이 글에서는 다루지 않겠습니다.

우선 ReactJS를 설치하기 전에 `npm` 과 `npx` 의 차이점에 대해 간단히 다루겠습니다.

### npm

node.js의 자동화 된 의존성과 패키지 관리를 위한 패키지 매니저입니다.

npm은 `-g` 옵션을 사용해 로컬의 글로벌한 공간에 모듈을 설치하여 프로젝트마다 같은 모듈을 공유해서 사용할 수 있습니다.

다만 이런 방법은 단점이 존재합니다.

**1. 모듈이 업데이트 되었는지, 안되었는지 확인이 불가능합니다.**

모든 프로젝트마다 모듈을 재설치 하는 것이 아닌, 한 번 설치한 모듈을 계속하여 사용하기 때문에 개발자가 모듈을 최신 버전으로 재 설치하지 않으면 확인 자체가 불가능합니다.

**2. 업데이트를 진행했을 때 변동사항이 생겨 다른 프로젝트에도 영향을 끼칠 수 있습니다.**

프로젝트를 여러 개 운영할 시, 같은 모듈의 각각 다른 버전이 필요한 상황이 있을 수 있습니다. 이 때 글로벌 모듈의 버전은 당연히 한 개로 통일되어 있기 때문에 문제가 발생할 수 있습니다.

**3. create-react-app 같은 보일러 플레이트에는 치명적입니다.**

리액트 프로젝트 생성 도구인 `create-react-app` 같은 모듈의 경우, 변경사항이 꽤 많은 모듈입니다. 때문에 매 설치 전마다 npm으로 재 설치를 하지 않은 경우 이전 버전을 사용할 여지가 많습니다.

이런 보일러플레이트 같은 경우에는 항상 최신 버전을 유지해 주는 것이 좋은데, 매번 설치하는 것이 꽤 귀찮은 일입니다.

이러한 단점 때문에 React 공식 문서에는 `npx` 를 사용하여 설치하는 것을 권장하고 있습니다.

### npx

npm 5.2.0 버전부터 추가된 node.js 패키지를 실행시키는 하나의 도구입니다.

npx는 npm을 통해 모듈을 로컬에 설치했어야만 실행시킬 수 있었던 기존 문제점을 해결해주었습니다. 모듈을 로컬에 저장하지 않고, 매번 최신 버전의 파일만을 임시로 불러와 실행 시킨 후에, 다시 그 파일은 없어지는 방식을 사용하기 때문에 더 효율적입니다.

### npm 사용하여 설치

``` bash
$ npm install -g create-react-app

# 위의 명령어가 에러가 발생할 경우 아래의 명령어 입력
$ sudo npm install -g create-react-app
```

> npm에게 Global하게 사용할 수 있도록 react를 설치해달라는 명령어

### npx 사용하여 설치 (권장)

``` bash
# 1. npx 설치
$ sudo npm install npx -g

# 2. react 설치
$ npx create-react-app
```



### React 실행

React 프로젝트가 담긴 디렉토리에서 아래의 명령어를 입력합니다.

``` bash
$ npm start
```

그러면 `localhost:3000` 이 실행되면서 아래의 이미지와 같이 React 프로젝트가 실행됩니다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDPU2S%2Fbtq11zHYthA%2FSEBEgnKJYssHKBmQbwmRj0%2Fimg.png">