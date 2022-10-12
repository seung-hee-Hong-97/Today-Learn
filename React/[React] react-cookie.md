# [React] react-cookie



## Cookie 란?

- 웹서버가 웹 브라우저에게 보내어 저장했다가, 서버의 부가적인 요청이 있을 때 다시 서버로 보내주는 문자열 정보
- 웹페이지 방문시 방문 기록 등 바라우저에서의 정보들이 저장된 텍스트 파일

웹에서 요청마다 매번 연결과 해제가 되면서 요청마다 새로운 사용자로 인식이 되는 단점이 있지만, 쿠키와 세션을 통해 브라우저를 종료했다가 다시 접속해도 로그인 상태를 유지할 수 있습니다.

쉽게 말해, 쿠키는 서버를 대신하여 웹 브라우저에 저장하고 요청을 할 때 그 정보를 서버에 보내 사용자를 식별할 수 있게 합니다.



## Cookie의 사용

쿠키는 세션관리, 개인화, 트래킹에 사용되며, 세션은 쿠키를 이용합니다.

웹 브라우저가 서버에 요청을 하면 서버는 세션 아이디를 할당해서 응답할 때 함께 전달합니다.

쿠키는 서버가 사용자의 웹 브라우저에 저장하며 데이터 형태는 Key, Value형태이고 4KB 이상 저장이 불가능합니다.



## Cookie 종류

- `기술적 쿠키` : 검색하는 주제가 사람인지 혹은 애플리케이션인지 구분 기능 수행
- `분석 쿠키` : 어떤 종류를 검색하는지, 많이 검색하는지, 시간, 언어 등의 정보를 수집
- `광고 쿠키` : 검색 내용, 국가, 언어에 따라 광고를 띄우는 기능 수행



## Cookie와 Session의 차이점

### `Cookie`

클라이언트가 서버로 요청을 보낸 뒤, 그에 대한 지속 필요정보를 서버에서 쿠키에 담아 보냅니다. 이에 클라이언트는 PC에 저장한 뒤 서버에 요청을 보낼 때마다 쿠키를 같이 보냅니다.

그렇기 때문에 쿠키에 대한 정보를 통신을 할 때마다 Header에 추가하여 보내기 때문에 상당한 트래픽을 발생시킵니다.

- 클라이언트에 정보가 저장되어 보안이 취약함
- 클라이언트에 저장되어 있기 때문에 브라우저를 종료해도 정보가 계속 남아 있음



### `Session`

서버가 클라이언트마다 개별의 세션 ID를 부여하고, 클라이언트는 요청할 때마다 세션 ID를 서버에 전달합니다.

서버는 받은 세션 ID로 클라이언트 정보를 가져와서 활용합니다.

- 쿠키보다 보안성이 뛰어남
- 쿠키에 비해 서버에서 부담하는 처리량이 늘어남
- 사용자가 많아지면 서버 측 부하가 늘어나기 때문에 쿠키보다 속도가 느림
- 브라우저가 종료되면 정보가 삭제됨



## react-cookie

`react-cookie`를 사용하기 위해선 아래의 명령어를 입력하여 설치를 진행합니다.

``` bash
$ npm install react-cookie
```



쿠키와 관련된 함수들로 구성된 파일을 `utils` 폴더 하위에 위치시킵니다.

### `Cookie.js`

``` javascript
import { Cookies } from 'react-cookie';

const cookies = new Cookies();

export const setCookie = () => {
  return cookies.set(name, value, {...options})
}

export const getCookie = () => {
  return cookies.get(name)
}
```

> set과 get 함수로 쿠키를 저장하고 이용할 수 있습니다.



### `CookiesProvider`

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { CookiesProvider } from 'react-cookie';

ReactDOM.render(
  <CookiesProvider>
    <App />
  </CookiesProvider>
  
  document.getElementById('root')
)
```