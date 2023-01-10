# Netlify를 통한 배포

Netlify를 활용하여 React 프로젝트를 배포하는 방법을 알아보겠습니다.



## 1. Netlify 접속 및 Sign Up

[Netlify 사이트](https://www.netlify.com/)에 접속하면 아래와 같은 화면이 보입니다.

<img src="https://user-images.githubusercontent.com/68320595/211260671-d06eb000-5b4c-41c2-8671-a64eba1a8ca2.png" />

> Get started for free 버튼을 클릭하여 로그인을 진행해줍니다.



버튼을 클릭하셨다면 본인이 원하는 플랫폼을 선택하여 로그인을 진행해주세요.

<img src="https://user-images.githubusercontent.com/68320595/211260892-cd1c6aba-7819-4cbe-885c-2f857112c06d.png" align="left" />

저는 GitHub를 선택하여 진행 했습니다. 본인이 원하는 플랫폼을 사용하면 되기 때문에 굳이 GitHub를 선택 할 필요는 없습니다.



## 2. Netlify와 Github Repository 연동

Sites > Add new site > import an exisiting project 순으로 진행합니다.

<img src="https://user-images.githubusercontent.com/68320595/211262577-e55d4c9d-1e80-4b08-baf5-4a0765479bef.png" />



위의 순서대로 진행했다면 아래와 같이 플랫폼을 선택하여 Git Repository를 연동하는 화면이 보일 것입니다.

<img src="https://user-images.githubusercontent.com/68320595/211262893-816bd0bc-28f4-466c-9d9f-f13cf17422a6.png" width="700" align="left"/>



본인의 프로젝트가 올라와 있는 플랫폼을 선택해주세요. 저는 GitHub를 선택했습니다.

이 때 해당 플랫폼과 연동을 위해 권한 요청을 하는 작업이 있을 수 있는데 모든 저장소를 연동하고 싶으면 `All repositories` 를 특정 저장소만 선택하여 연동하고 싶으면 `Only select repositories` 를 선택하면 됩니다.

선택을 하고 나면 연동되어 있는 저장소 목록들이 아래의 이미지처럼 보일 것 입니다.

<img src="https://user-images.githubusercontent.com/68320595/211263527-8f8464cb-90a2-4993-bee4-57bb5e50707f.png" width="450" height="550" align="left" />

> 자신이 배포할 프로젝트가 있는 Repository를 선택합니다.

Repository를 선택했다면 아래의 이미지와 같이 배포할 Branch 및 빌드 세팅 하는 화면이 보일 것 입니다.

<img src ="https://user-images.githubusercontent.com/68320595/211460404-469d0a4d-e093-4f2e-8402-f381e0c365c0.png" width="450" align="left" />

> - `Branch to deploy` : 본인이 배포할 코드가 있는 브랜치 선택
> - `Build command` : 프로젝트를 빌드할 때 사용한 빌드 커맨더를 입력
>   - `yarn 사용` : yarn build
>   - `npm 사용` : CI= npm run build
> - `Publish directory` : 빌드 커맨더를 입력 후 생성 된 `build` 파일의 경로 입력



### 환경 변수 사용 했을 경우 추가적인 Build Settings

본인이 작업했던 프로젝트에서 `.env` 파일에 환경 변수를 선언하여 사용했다면 Netlify를 통해서 배포할 때 빌드 설정에 환경 변수를 추가해줘야 합니다.

위의 이미지에서 빌드 세팅 아래 부분에 `Show advanced` 버튼을 클릭하면 `Environment variables` 및 `Functions settings`가 보일 것 입니다.

<img src="https://user-images.githubusercontent.com/68320595/211461898-bc003d5f-2269-42fa-8fbf-9d4dc1d1a924.png" width="450" align="left"/>

> `New variable` 버튼을 눌러 환경 변수를 추가해줍니다.

<img src="https://user-images.githubusercontent.com/68320595/211462163-4d5b5ff9-eefa-4fbd-8908-45d52f1f17aa.png" width="450" align="left" />



여기까지 모든 설정을 맞췄다면 `Deploy` 버튼을 클릭하여 사이트 배포를 진행하면 됩니다.



## 3. 사이트 이름 변경

지금까지 잘 따라왔다면 정상적으로 사이트 배포가 됐을 것입니다.

만약 배포에 실패했다면 에러 로그를 구글링하여 해결하시고 다시 빌드 해보시길 바랍니다.

사이트 배포가 완료되었다면 아래와 같이 프로젝트 최초 페이지가 보이면서 사이트 주소가 보일 것 입니다.

<img src="https://user-images.githubusercontent.com/68320595/211463154-4a9cabe4-1516-45f7-b1f6-b3ff7f0d4440.png" width="450" align="left" />

위의 이미지에 나와 있는 사이트 URL은 제가 변경한 것이기에 짧고 정돈 되었지만 최초 URL은 `https://434adff-3zcvvk9.netlify.app` 처럼 깔끔하지 못한 URL을 제공합니다.

따라서 사이트 이름을 변경 해주도록 하겠습니다.

`Site settings > Site details > Change site name` 

<img src="https://user-images.githubusercontent.com/68320595/211464241-88dc1b24-fbbf-44a1-aa0c-a9382f2d58e7.png" width="800" align="left" />

Change site name 버튼을 클릭하여 본인이 원하는 사이트 이름으로 변경합니다.

당연하게도 이미 존재하는 사이트 이름으로는 바꿀 수 없습니다.

<img src="https://user-images.githubusercontent.com/68320595/211464494-83ae1b27-995b-430a-a837-3c0414df4990.png" width="500" align="left" />

중복 되지 않고 본인이 원하는 이름으로 변경하였다면 `Save` 버튼을 클릭합니다.

다시 사이트 URL을 살펴보면 정상적으로 바뀐 것을 확인할 수 있습니다.



















