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

`GET`

- 데이터 조회
- 서버에서 어떤 데이터를 가져와서 보여줌

`POST`

- 데이터 등록
- 서버로 데이터를 보냄

`PUT`

- 데이터 수정
- DB 내부 내용 갱신 (업데이트)

`DELETE`

- 데이터 제거
- DB 내부 내용 삭제



### GET

``` javascript
import axios from 'axios';

async function getData() {
  try {
  // 응답 성공
  const response = await axios.get('URL 주소');
  // 1. 받아온 정보 전부를 콘솔에 찍는 경우
  console.log(response);
  
  // 2. 받아온 정보에서 데이터만 콘솔에 찍는 경우
  console.log(response.data);
  } catch (error) {
    // 응답 실패
    throw new Error(error);
  }
}
```

> `get` 메소드 내부에 API의 주소를 넣어주면 됩니다.



### POST

서버에 데이터를 보내는 `POST` 메소드에서는 보내고자 하는 데이터를 `message body` 에 포함시켜 보냅니다.

```javascript
import axios from 'axios';

async function postData() {
  try {
    // 응답 성공
    const response = await axios.post('URL 주소', {
      // 보내고자 하는 데이터
      username: 'kim',
      password: '1234'
    });
    console.log(response);
  } catch (error) {
    // 응답 실패
    throw new Error(error);
  }
}
```

> `post` 메소드를 통해 데이터를 등록할 때 두번째 파라미터에 등록하고자 하는 정보를 넣을 수 있습니다.



### PUT

DB의 내용을 수정하는 `PUT` 메소드는 서버 내부적으로 `get` -> `post` 하는 과정을 거치기 때문에 `POST` 메소드와 비슷한 형식입니다.

``` javascript
import axios from 'axios';

async function putData() {
  try {
    // 응답 성공
    const response = await axios.put('URL 주소', {
      // 항목에 새롭게 넣을 데이터
      username: 'lee',
      password: '4561'
    });
    console.log(response);
  } catch (error) {
    // 응답 실패
    throw new Error(error);
  }
}
```



### DELETE

DB 내부의 데이터를 삭제하는 `DELETE` 메소드는 일반적으로는 **body가 비어있는 형태** 이지만, query나 params가 많아서 **헤더에 많은 정보를 담기 어려운 상황에서는** 두번째 인자에 data를 추가합니다.

``` javascript
// 1. 일반적인 delete
import axios from 'axios';

async function deleteData() {
  try {
    // 응답 성공
    const response = await axios.delete('URL 주소');
    console.log(response);
  } catch (error) {
    // 응답 실패
    throw new Error(error);
  }
}

// 2. 헤더에 많은 정보가 포함된 경우
import axios from 'axios';

async function deleteData() {
  try {
    // 응답 성공
    const response = await axios.delete('URL 주소', {
      // 헤더에 포함된 정보들
      data: {
        post_id: 1,
        comment_id: 10,
        username: 'park'
      }
    });
    
    console.log(response);
  } catch (error) {
    // 응답 실패
    throw new Error(error);
  }
}
```





## 3. axios 활용한 API 연동 실습

API 연동 실습을 위한 테스트 데이터 URL

`https://jsonplaceholder.typicode.com/users`

```javascript
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

