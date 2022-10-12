# [React] JWT 로그인

- `JWT(JSON-WEB-Token)`에는 `accessToken` 과 `refreshToken`이 존재하며 유저 인증에 사용됩니다.

- 실질적인 인증 정보는 `accessToken`에 있지만 일정 시간이 지나면 만료하는 구조를 가지고 있습니다.

- `refreshToken`,  `accessToken`을 클라이언트에 저장해두는데 이 떄, `refreshToken`을 이용하면 로그인은 지속적으로 유지할 수 있습니다. 이 구조는 `refreshToken`을 서버에 보내면 그 때마다 새로운 `accessToken`을 발급하여 돌려주는 방식입니다.

- `accessToken`을 필요할 때 서버에 보내면 서버는 토큰이 유효한지 확인합니다.

> 전체적인 JWT를 이용한 로그인 과정입니다.



## Token

- `refreshToken` : `Secure` & `HttpOnly` 라는 쿠키 옵션으로 전달 받습니다.
- `accessToken` : JSON payload로 전달 받습니다.

> - `Secure` : 웹 브라우저와 웹 서버가 https로 동신하는 경우에만 웹 브라우저가 쿠키를 서버로 전송하는 옵션
> - `HttpOnly`c: Javascript의 document.cookie를 이용해서 쿠키에 접속하는 것을 막는 옵션



## 로그인 방식

### `(1) 세션 ID를 이용하는 방식`

1. 유저가 로그인 시도
2. 서버에서 세션 생성
3. 생성된 세션의 id를 클라이언트에 보냄
4. 클라이언트는 받은 세션 id를 클라이언트에 저장
5. 인증이 필요한 데이터를 가져올 때 서버에 세션 id값을 보냄
6. 서버는 받은 ID를 통해 세션을 불러와 유효한지 확인



### `(2) JWT를 이용하는 방식`

1. 유저가 로그인 시도
2. 서버가 암호화나 시그니처 추가가 가능한 데이터 패키지 안에 인증 정보를 담아서 보냄
3. 담기는 정보 중 `accessToken`, `refreshToken`이 이후 유저 인증에 사용됨
4. 위의 정보를 클라이언트에 저장
5. `accessToken`을 유저에게만 보여줄 수 있는 정보에 접근할 때 서버에 보냄
6. 서버는 그 토큰이 유효한지 확인



## 브라우저 저장소 종류

### `(1) localStorage`

브라우저 저장소에 저장하는 방식이고, Javascript 내 글로벌 변수로 읽기 / 쓰기 접근이 가능합니다.



### `(2) 쿠키 저장`

브라우저에 쿠키로 저장되고, 클라이언트가 HTTP 요청을 보낼 때마다 자동으로 쿠키가 서버에 전송됩니다.

Javascript 내 글로벌 변수로 읽기 / 쓰기 접근이 가능합니다.



### `(3) Secure & HttpOnly 저장`

브라우저에 쿠키로 저장되는 것은 동일하지만, Javascript 내에서 접근이 불가능합니다.

`Secure` 을 적용하면 Https 접속에서만 동작합니다.



## React에서 JWT를 활용한 로그인 구현 예제

`Secure` 쿠키 전달을 하기 위해선 프론트와 로그인 API를 제공할 백엔드(서버 API)는 같은 도메인을 공유해야 합니다.

백엔드는 HTTP Response `Set-Cookie` 헤더에 `refreshToken` 값을 설정하고 `accessToken`을 JSON payload에 담아 보내줘야 합니다.

``` javascript
import axios from 'axios';

axios.defaults.baseURL = 'https://www.example.com';
axios.defaults.withCredentials = true;
```

> `index.js` 파일의 최상단에서 `axios`의 `withCredentials` 를 true로 설정해야 `refreshToken` 쿠키를 주고 받을 수 있습니다.



``` javascript
const onLogin = async(email, password) => {
  const data = {
    email,
    password,
  };
  
  await axios.post('/login', data).then(response => {
    const { accessToken } = response.data;
    
    // API를 요청할 때 마다 헤더에 accessToken을 담아서 보내도록 설정
    axios.defaults.headers.common['Authorization'] = `Token : ${accessToken}`;
  }).catch(error => {
    // Error Code..
  });
}
```