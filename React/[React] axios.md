# [React] axios

React 프로젝트에서 `axios` 를 사용하여 API 연동하는 방법을 알아보겠습니다.



## 1. axios 설치

`npm` 혹은 `yarn`을 통해 axios를 설치할 수 있습니다.

이 글에선 `npm` 을 통해서 설치할 것이고, 터미널을 열어 아래의 명령어를 입력하여 설치합니다.

``` bash
$ npm install axios
```



## 2. axios 메소드 및 사용법

`axios` 를 사용해서 GET, POST, PUT, DELETE 등의 메소드를 통해 API 요청을 할 수 있습니다.

- `GET` : 데이터 조회
- `POST` : 데이터 등록
- `PUT` : 데이터 수정
- `DELETE` : 데이터 제거



### GET

``` javascript
import axios from 'axios';

axios.get('/users/1');
```

> `get` 메소드 내부에 API의 주소를 넣어주면 됩니다.



### POST

```javascript
import axios from 'axios';

axios.post('/users', {
  username: 'lalala',
  name: 'lalala',
});
```

> `post` 메소드를 통해 데이터를 등록할 때 두번째 파라미터에 등록하고자 하는 정보를 넣을 수 있습니다.



## 3. axios 활용한 API 연동 실습

API 연동 실습을 위한 테스트 데이터 URL

`https://jsonplaceholder.typicode.com/users`

```react
import axios from 'axios';
import { useState, useEffect } from 'react';

const [loading, setLoading] = useState(true);
const [mocks, setMocks] = useState([]);

const getData = async() => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/users');
    setMocks(response.data);
  } catch (e) {
    throw new Error(e);
  }
  
  setLoading(false);
}

useEffect(() => {
  getData();
}, []);

function App() {
  return (
    <div>
      {loading ? <h1>Loading...</h1> : (
        <div>
        {mocks.map((mock) => (
          <div>
            <p>name: {mock.name}</p>
            <p>userName: {mock.username}</p>
            <p>email: {mock.email}</p>
          </div>
        ))}
        </div>
       )}
    </div>
  )
}

export default App;
```

